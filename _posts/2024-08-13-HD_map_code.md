---
layout: single
title: "[HD_Map] Localization Code - 진행중" 
categories: HD_Map
typora-root-url: ../
toc: true
---



## 환경설정

이미지와 GPS 값이 존재하는 상황

### 이미지 불러오기

```html
# 엔드포인트: /image 경로, 이미지 요청 처리
@app.route('/send-image', methods=['POST'])
def send_image():
    data = request.json
    # image_id 값 가져오기
    image_time = data['image_time']
    image_time_replace= image_time.replace('-', '_').replace(':', '_').replace(' ', '_')
    formatted_time = image_time_replace[:4] + '_' + image_time_replace[5:7] + image_time_replace[8:10] + '_' + image_time_replace[11:13] + image_time_replace[14:16] + '_' + image_time_replace[17:]

    folder_path = os.path.join(current_dir,'data_image')
    # 이미지 파일 리스트를 시간 순서대로 가져오기
    image_files = get_image_files_sorted_by_time(folder_path)

    # 필터 시간에 가장 가까운 이미지의 시작 인덱스 찾기
    start_index = find_start_index(image_files, formatted_time)
    

    if start_index == -1:
        print("No matching images found for the given filter time.")
    else:
        # 이미지 로딩 제너레이터 생성
        image_loader = load_image_by_index(image_files, start_index)
        
        for image_file in image_loader:
            # 이미지 파일을 불러와 처리

            # 실제로는 이미지 파일을 열고 처리하는 코드가 필요함
            # 예: image = open_image(os.path.join(folder_path, image_file))
            break  # 실제 키보드 이벤트로 반복을 제어

    # 서버의 이미지 파일 경로 지정
    image_path = os.path.join(current_dir,'data_image', image_file)
    
    if not os.path.exists(image_path):
        return "Image not found", 404
    
    # 이미지 파일을 클라이언트에 전송
    return send_file(image_path, mimetype='image/jpeg')
```





## 알고리즘



100 frame 이전 : 

​	연산량을 위해 초기 100frame 간 초반 노드/링크 후부군 제시

1. 현재 위치를 받아온다. [code](#get_next_coordinates)
2. 현재 위치를 기준으로 근접 노드 10개 찾기[code](#find_closest_nodes)
3. 근접 노드로부터 연결된 Link를 찾기[code](#find_links_with_node_id)
4. 차량의 Heading을 기준으로 후보군 링크 정의[code](#select_candidate_links)



100frame 이후

​	이전에 후보군으로 정의한 후보군 링크를 기반으로 작업

1. '현재 좌표'와 '후보군 링크'와의 비교를 통한 현재 링크 추정
2. '현재 링크'와 '후보군 링크'를 비교해서 왼쪽 오른쪽 차선을 검색





### 현재 위치를 받아오는 코드 {#get_next_coordinates}

```python
def get_next_coordinates():
    """
    Returns the next set of coordinates from the NMEA JSON data.

    Returns:
        dict: A dictionary containing the following keys:
            - "time" (str): Timestamp of the coordinate.
            - "latitude" (float): Latitude value.
            - "longitude" (float): Longitude value.
            - "altitude" (float): Altitude value.
    """
    global current_index
    nmea_json = read_nmea(fr"{current_dir}\data\{nmea_file}") ## nmea 파일을 불러온다.
    coordinate = nmea_json[current_index]					  ## 불러온 json 파일을 인덱스에 따라 하나씩 읽어오기
    current_index = (current_index + 1) % len(nmea_json)  	  ## 함수 호출이 될 때마다 current_index 증가 
    														  ## current_index가 nmea_json의 길이를 넘어가면 0으로 초기화

    return coordinate										
```



###  현재 위치를 기준으로 근접 노드 10개 찾기 {#find_closest_nodes}

```python
def find_closest_nodes(target_coord, num_closest=10):
    """
    주어진 좌표에 가장 가까운 노드들을 찾는 함수.

    Args:
        target_coord (dict): 목표 지점의 좌표를 포함한 딕셔너리로, 'latitude'와 'longitude' 키를 포함.
        num_closest (int, optional): 찾을 가까운 노드의 수. 기본값은 10.

    Returns:
        list: 가장 가까운 노드들의 정보를 담은 리스트. 각 노드 정보는 'node_id', 'node_lat', 'node_lon'을 포함하는 딕셔너리.
    """
    
    distance_lane = []  # 각 노드와의 거리를 저장할 리스트
    target_lat_lon = (target_coord['latitude'], target_coord['longitude'])  # 목표 지점의 (위도, 경도) 튜플 생성

    # 각 노드와의 거리를 계산하여 리스트에 추가
    for i in range(len(map_node['features'])):
        origin_coordinate = map_node['features'][i]['geometry']['coordinates']  # 노드의 [경도, 위도, 고도] 좌표 추출
        change_coordinate = [origin_coordinate[1], origin_coordinate[0]]  # 좌표를 [위도, 경도] 순서로 변경
        distance = haversine_distance(target_lat_lon, change_coordinate)  # 목표 지점과 노드 간의 거리 계산
        distance_lane.append((distance, i))  # (거리, 노드 인덱스) 형태로 리스트에 추가

    # 거리 순서대로 정렬하여 가까운 순서대로 나열
    distance_lane.sort(key=lambda x: x[0])

    nodes_info = []  # 가장 가까운 노드들의 정보를 저장할 리스트
    for _, index in distance_lane[:num_closest]:  # 가장 가까운 num_closest개의 노드에 대해 반복
        node = map_node['features'][index]  # 해당 인덱스의 노드 정보 가져오기
        id_node = node['properties']['ID']  # 노드의 ID 추출
        closest_node = node['geometry']['coordinates']  # 노드의 좌표 [경도, 위도, 고도] 추출
        
        nodes_info.append({
            'node_id': id_node,  			# 노드 ID 저장
            'node_lat': closest_node[1],    # 위도 저장
            'node_lon': closest_node[0]     # 경도 저장
        })

    return nodes_info  # 가장 가까운 노드들의 정보를 담은 리스트 반환
```



### 근접 노드로부터 연결된 Link를 찾기 {#find_links_with_node_id}

```python
def find_links_with_node_id(node_info):
    """
    가까운 노드들과 연결된 링크들을 찾는 함수.

    Args:
        node_info (list): 
            - 'find_closest_nodes' 함수로부터 반환된 노드 정보 리스트.
            - 리스트의 각 항목은 'node_id', 'node_lat', 'node_lon'을 포함하는 딕셔너리.

    Returns:
        list: 
            - 노드와 연결된 링크들의 정보를 담은 리스트.
            - 각 링크 정보는 'from_node_id', 'to_node_id', 'link_id', 'link_length', 'link_geometry', 'link_heading'을 포함하는 딕셔너리.
    """
    
    links_info = []  # 링크 정보를 저장할 리스트

    # 모든 링크에 대해 반복
    for i in range(len(map_link['features'])):
        link = map_link['features'][i]
        properties = link['properties']
        from_node_id = properties['FromNodeID']  # 링크 시작 노드 ID
        to_node_id = properties['ToNodeID']      # 링크 끝 노드 ID

        # 가까운 노드 리스트에서 현재 링크의 노드와 일치하는지 확인
        for node in node_info:
            node_id = node['node_id']
            if node_id == from_node_id or node_id == to_node_id:
                # 링크의 시작점과 끝점 좌표 추출
                start_coord = link['geometry']['coordinates'][0]  # 시작점 좌표
                end_coord = link['geometry']['coordinates'][-1]   # 끝점 좌표
                
                # 시작점과 끝점을 통해 방향(heading) 계산
                heading = calculate_heading(start_coord, end_coord)

                # 링크 정보를 딕셔너리로 저장
                link_info_dict = {
                    'lane_num': properties['LaneNo'],           # 차선 수
                    'from_node_id': from_node_id,               # 시작 노드 ID
                    'to_node_id': to_node_id,                   # 끝 노드 ID
                    'link_id': properties['ID'],                # 링크 ID
                    'link_length': properties['Length'],        # 링크 길이
                    'link_geometry': link['geometry']['coordinates'],  # 링크의 좌표들
                    'link_heading': heading                     # 링크의 방향(heading)
                }
                
                # 링크 정보를 리스트에 추가
                links_info.append(link_info_dict)

    return links_info  # 링크 정보 리스트 반환
```



### 후보 Link 찾기 {#select_candidate_links}

```python
def select_candidate_links(links_info, current_heading, threshold=10):
    """
    현재 차량의 heading을 기준으로 주행 중인 링크들을 선택하는 함수.

    Args:
        links_info (list): 
            - 'find_links_with_node_id' 함수로부터 반환된 링크 정보 리스트.
        current_heading (float): 
            - 현재 차량의 heading 값.
        threshold (int, optional): 
            - 허용하는 heading 차이 범위 (기본값은 ±10도).

    Returns:
        list: 
            - 주행 중일 가능성이 높은 링크들의 리스트.
    """
    selected_candidate_links = []  # 선택된 링크들을 저장할 리스트
    
    for link in links_info:
        link_heading = link['link_heading']  # 링크의 heading 값
        
        # 현재 heading과 링크 heading의 차이 계산
        heading_difference = abs(current_heading - link_heading)
        
        # 각도 차이가 180도를 넘지 않도록 조정 (360도 기준)
        heading_difference = min(heading_difference, 360 - heading_difference)
        
        # heading 차이가 threshold 이하인 링크를 선택
        if heading_difference <= threshold:
            selected_candidate_links.append(link)
    
    return selected_candidate_links
```



---

### 현재 링크 추정 {#search_current_link}

```Python
def search_current_link(current_location, links_info, group_size=10, step_size=10):
    """
    현재 위치와 가장 가까운 링크를 찾고 해당 링크의 모든 정보를 반환하는 함수.

    Args:
        current_location (dict): 
            - 현재 위치를 나타내는 딕셔너리로, 'latitude'와 'longitude' 키를 포함.
        links_info (list): 
            - 링크 정보를 담은 리스트로, 각 링크는 'link_id', 'link_geometry', 'link_length' 등의 정보를 포함.
        group_size (int, optional): 
            - 링크를 군집화할 때 사용할 그룹 크기. 기본값은 10.
        step_size (int, optional): 
            - 군집화할 때 슬라이딩 윈도우의 스텝 크기. 기본값은 10.

    Returns:
        dict: 
            - 현재 위치에서 가장 가까운 링크의 모든 정보.
    """
    
    # 1. 링크들을 군집화하여 대표 좌표를 얻는다.
    representative_coords = get_representative_coordinates(links_info, group_size, step_size)
    
    # 2. 현재 위치와 가장 가까운 대표 좌표를 찾는다.
    closest_distance = float('inf')
    closest_link = None

    current_lat_lon = (current_location['latitude'], current_location['longitude'])

    for rep in representative_coords:
        rep_lat_lon = (rep['lat'], rep['lon'])
        distance = haversine_distance(current_lat_lon, rep_lat_lon)

        if distance < closest_distance:
            closest_distance = distance
            closest_link = rep['link_id']
    
    # 3. 가장 가까운 링크의 정보를 반환
    if closest_link is not None:
        for link in links_info:
            if link['link_id'] == closest_link:
                print('here :', closest_link)
                return link
    
    return None  # 만약 가장 가까운 링크가 없을 경우 None 반환
```



### 주행 가능 영역인 양쪽 차선 검증 {#find_drivable_lanes}

```Python
def find_drivable_lanes(closest_link, candidate_links):
    """
    주어진 후보군 링크에서 가장 가까운 링크를 기반으로 주행 가능한 차선을 찾는 함수.

    Args:
        closest_link (str): 가장 가까운 링크의 ID.
        candidate_links (list): 후보군 링크 정보를 담은 리스트.

    Returns:
        list: 주행 가능한 차선의 전체 정보 (링크의 모든 속성과 좌표 포함).
    """
    drivable_lanes = []
    visited_links = set()  # 방문한 링크를 저장하기 위한 집합

    def search_lane(link_id, direction):
        while link_id and link_id not in visited_links:
            visited_links.add(link_id)
            link = next((l for l in candidate_links if l['properties']['ID'] == link_id), None)
            if not link:
                break
            drivable_lanes.append(link)  # 전체 링크 정보를 추가
            link_id = link['properties'][direction]

    # 후보군 링크를 검색
    for link in candidate_links:
        if link['properties']['ID'] == closest_link:
            # 현재 링크 정보에서 좌/우 차선 ID를 가져옴
            left_link_id = link["properties"]["L_LinKID"]
            right_link_id = link["properties"]["R_LinkID"]

            print(f"Left link ID: {left_link_id}, Right link ID: {right_link_id}")

            drivable_lanes.append(link)  # 전체 링크 정보를 추가

            # 왼쪽 차선 검색
            search_lane(left_link_id, 'L_LinKID')
            # 오른쪽 차선 검색
            search_lane(right_link_id, 'R_LinkID')

            break  # closest_link는 유일하므로 찾으면 루프 종료

    return drivable_lanes
```

