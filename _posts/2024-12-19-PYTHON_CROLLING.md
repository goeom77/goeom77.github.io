---
layout: post
title:  " [Python] 크롤링 개발(네이버 지도에서 플레이스 추출) "
categories: Crolling
author: start-easy
---
* content
{:toc}

* 작성 중

## 크롤링

* 크롤링을 하기 위해서 python, html, css를 이해하는 것이 선제되어야 합니다.

* (주의) 밑의 코드는 네이버 블로그를 크롤링 한 파일입니다. 네이버에서 내부 클래스 명이 자주 바뀌므로 동작하지 않을 수 있습니다.

* 필요성 네이버 지도를 통한 데이터를 가져올때 모든 데이터를 하나씩 입력하는 것이 오랜 시간이 걸립니다.

* 또한 데이터를 다양하게 수집해 필터링 한다면 데이터를 유용하게 관리할 수 있다는 판단하에 시작하게 되었습니다.

---

* 우선 검색어를 필터링 하기 위해 시도 정보를 가져옵니다.

* 시도 정보 가져오기

```shell
import requests
import json
import xml.etree.ElementTree as ET
import os
url = 'http://api.vworld.kr/req/data'
params = {
    'service': 'data',
    'request': 'GetFeature',
    'data': 'LT_C_ADSIDO_INFO',
    'key': "개인 키",
    'geometry': 'false',
    'size': '100',
    'geomFilter': 'BOX(124,33,132,43)'
}
try:
    # API 요청 보내기
    response = requests.get(url, params=params)
    response.raise_for_status()  # HTTP 에러 발생 시 예외 발생

    # JSON 데이터 파싱
    data = response.json()

    # ctp_kor_nm 값 추출하여 리스트로 저장
    ctp_kor_nm_list = [feature['properties']['ctp_kor_nm'] for feature in data['response']['result']['featureCollection']['features']]

    # 결과 출력
    print(ctp_kor_nm_list)

except requests.exceptions.RequestException as e:
    print('API 요청 중 오류 발생:', e)
except KeyError as e:
    print('JSON 데이터 파싱 중 오류 발생 - 필드가 존재하지 않음:', e)
except Exception as e:
    print('오류 발생:', e)
root = ET.Element("data")

try:
    for ctp_kor_nm in ctp_kor_nm_list:
        # API 요청 파라미터 설정
        params = base_params.copy()
        params['attrFilter'] = f"full_nm:like:{ctp_kor_nm}"

        # API 요청 보내기
        response = requests.get(url, params=params)
        response.raise_for_status()  # HTTP 에러 발생 시 예외 발생

        # JSON 데이터 파싱
        data = response.json()

        # XML에 데이터 추가
        for feature in data['response']['result']['featureCollection']['features']:
            item = ET.SubElement(root, "item")
            ctp_kor_nm_elem = ET.SubElement(item, "ctp_kor_nm")
            ctp_kor_nm_elem.text = ctp_kor_nm
            sig_kor_nm = ET.SubElement(item, "sig_kor_nm")
            sig_kor_nm.text = feature['properties']['sig_kor_nm']

except requests.exceptions.RequestException as e:
    print('API 요청 중 오류 발생:', e)
except KeyError as e:
    print('JSON 데이터 파싱 중 오류 발생 - 필드가 존재하지 않음:', e)
except Exception as e:
    print('오류 발생:', e)
else:
    # XML 파일 저장
    desktop_path = os.path.expanduser("~/Desktop")
    xml_filename = os.path.join(desktop_path, "sigg_data.xml")
    tree = ET.ElementTree(root)
    tree.write(xml_filename, encoding='utf-8', xml_declaration=True)
    print(f"XML 파일이 {xml_filename}에 저장되었습니다.")
# ctp_kor_nm_list를 저장할 sido.xml 파일 생성 및 저장
sido_root = ET.Element("sido")

for ctp_kor_nm in ctp_kor_nm_list:
    ctp_kor_nm_elem = ET.SubElement(sido_root, "ctp_kor_nm")
    ctp_kor_nm_elem.text = ctp_kor_nm

sido_xml_filename = os.path.join(desktop_path, "sido.xml")
sido_tree = ET.ElementTree(sido_root)
sido_tree.write(sido_xml_filename, encoding='utf-8', xml_declaration=True)
print(f"XML 파일이 {sido_xml_filename}에 저장되었습니다.")
```


### 크롤링 시작하기

* 가져온 시도 정보를 이용해 키워드를 조합하고 크롤링을 시작합니다.

* 목표는 필터링을 통해 리뷰순으로 만들고, 장소들 정보를 excel로 만드는 것입니다.

* 다만 이 글에는 장소들 정보를 가져오는 것만 기재되어 있습니다.


``` shell
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.action_chains import ActionChains

from time import sleep
import random
import re

from selenium import webdriver
import sys
    
options = webdriver.ChromeOptions()
options.add_argument('user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3')
options.add_argument('window-size=1380,900') ## 윈도우 사이즈 설정
driver = webdriver.Chrome(options=options)

# 대기 시간
driver.implicitly_wait(time_to_wait=3)

# 반복 종료 조건
loop = True

# 네이버 지도 페이지 접속
driver.get("https://map.naver.com")
time.sleep(3)  # 페이지 로딩 대기

# 검색창 찾기
search_box = driver.find_element(By.CSS_SELECTOR, "input.input_search")
searchList = "강서구 카페"  # 이것은 리스트로 넣어서 따로 for문을 돌려도 됩니다.
search_box.send_keys(searchList)
search_box.send_keys(Keys.RETURN)

# 검색 결과 로딩 대기
time.sleep(1)

def switch_left():
    ############## iframe으로 왼쪽 포커스 맞추기 ##############
    driver.switch_to.parent_frame()
    iframe = driver.find_element(By.XPATH,'//*[@id="searchIframe"]')
    driver.switch_to.frame(iframe)
    
def switch_right():
    ############## iframe으로 오른쪽 포커스 맞추기 ##############
    a = 3
    try:
        if a > 0:
            driver.switch_to.parent_frame()
            iframe = driver.find_element(By.XPATH,'//*[@id="entryIframe"]')
            driver.switch_to.frame(iframe)
        else:
            time.sleep(1)
    except:
        time.sleep(1)
        switch_right()
        

while(True):
    switch_left()

    ############## 맨 밑까지 스크롤 ##############
    scrollable_element = driver.find_element(By.CLASS_NAME, "Ryr1F")

    last_height = driver.execute_script("return arguments[0].scrollHeight", scrollable_element)

    while True:
        # 요소 내에서 아래로 600px 스크롤
        driver.execute_script("arguments[0].scrollTop += 600;", scrollable_element)

        # 페이지 로드를 기다림
        sleep(1)  # 동적 콘텐츠 로드 시간에 따라 조절

        # 새 높이 계산
        new_height = driver.execute_script("return arguments[0].scrollHeight", scrollable_element)

        # 스크롤이 더 이상 늘어나지 않으면 루프 종료
        if new_height == last_height:
            break

        last_height = new_height


    ############## 현재 page number 가져오기 - 1 페이지 ##############

    page_no = driver.find_element(By.XPATH,'//a[contains(@class, "mBN2s qxokY")]').text

    # 현재 페이지에 등록된 모든 가게 조회
    # 첫페이지 광고 2개 때문에 첫페이지는 앞 2개를 빼야함
    if(page_no == '1'):
        elemets = driver.find_elements(By.XPATH,'//*[@id="_pcmap_list_scroll_container"]//li')[2:]
    else:
        elemets = driver.find_elements(By.XPATH,'//*[@id="_pcmap_list_scroll_container"]//li')

    print('현재 ' + '\033[95m' + str(page_no) + '\033[0m' + ' 페이지 / '+ '총 ' + '\033[95m' + str(len(elemets)) + '\033[0m' + '개의 가게를 찾았습니다.\n')

    for index, e in enumerate(elemets, start=1):
        final_element = e.find_element(By.CLASS_NAME,'CHC5F').find_element(By.XPATH, ".//a/div/div/span")

        print(str(index) + ". " + final_element.text)

    print("-"*50)

    switch_left()

    sleep(2)

    for index, e in enumerate(elemets, start=1):
        store_name = '' # 가게 이름
        category = '' # 카테고리
        new_open = '' # 새로 오픈
        
        rating = 0.0 # 평점
        visited_review = 0 # 방문자 리뷰
        blog_review = 0 # 블로그 리뷰
        store_id = '' # 가게 고유 번호
        
        address = '' # 가게 주소
        business_hours = [] # 영업 시간
        phone_num = '' # 전화번호

        switch_left()


        # 순서대로 값을 하나씩 클릭
        e.find_element(By.CLASS_NAME,'CHC5F').find_element(By.XPATH, ".//a/div/div/span").click()

        sleep(2)

        switch_right()

        ################### 여기부터 크롤링 시작 ##################
        title = driver.find_element(By.XPATH,'//div[@class="zD5Nm undefined"]')
        store_info = title.find_elements(By.XPATH,'//div[@class="YouOG DZucB"]/div/span')


        # 가게 이름
        store_name = title.find_element(By.XPATH,'.//div[1]/div[1]/span[1]').text

        # 카테고리
        category = title.find_element(By.XPATH,'.//div[1]/div[1]/span[2]').text

        if(len(store_info) > 2):
            # 새로 오픈
            new_open = title.find_element(By.XPATH,'.//div[1]/div[1]/span[3]').text


        ###############################

        review = title.find_elements(By.XPATH,'.//div[2]/span')

        # 인덱스 변수 값
        _index = 1

        # 리뷰 ROW의 갯수가 3개 이상일 경우 [별점, 방문자 리뷰, 블로그 리뷰]
        if len(review) > 2:
            rating_xpath = f'.//div[2]/span[{_index}]'
            rating_element = title.find_element(By.XPATH, rating_xpath)
            rating = rating_element.text.replace("\n", " ")

            _index += 1

        try:
            # 방문자 리뷰
            visited_review = title.find_element(By.XPATH,f'.//div[2]/span[{_index}]/a').text


            # 인덱스를 다시 +1 증가 시킴
            _index += 1

            # 블로그 리뷰
            blog_review = title.find_element(By.XPATH,f'.//div[2]/span[{_index}]/a').text
        except:
            print('------------ 리뷰 부분 오류 ------------')

        # 가게 id
        store_id = driver.find_element(By.XPATH,'//div[@class="flicking-camera"]/a').get_attribute('href').split('/')[4]

        # 가게 주소
        address = driver.find_element(By.XPATH,'//span[@class="LDgIH"]').text


        try:
            driver.find_element(By.XPATH,'//div[@class="y6tNq"]//span').click()

            # 영업 시간 더보기 버튼을 누르고 2초 반영시간 기다림
            sleep(2)


            parent_element = driver.find_element(By.XPATH,'//a[@class="gKP9i RMgN0"]')
            child_elements = parent_element.find_elements(By.XPATH, './*[@class="w9QyJ" or @class="w9QyJ undefined"]')

            for child in child_elements:
                # 각 자식 요소 내에서 클래스가 'A_cdD'인 span 요소 찾기
                span_elements = child.find_elements(By.XPATH, './/span[@class="A_cdD"]')

                # 찾은 span 요소들의 텍스트 출력
                for span in span_elements:
                    business_hours.append(span)
            
            # 가게 전화번호
            phone_num = driver.find_element(By.XPATH,'//span[@class="xlx7Q"]').text

        except:
            print(print('------------ 영업시간 / 전화번호 부분 오류 ------------'))
        
        print(f'{index}. ' + str(store_name)' · ' + str(category) + str(new_open))
        print('평점 ' + str(rating) + ' / ' + visited_review + ' · ' + blog_review)
        print(f'가게 고유 번호 -> {store_id}')
        print('가게 주소 ' + str(address))
        print('가게 영업 시간')
        for i in business_hours:
            print(i.text)
            print('')
        print('가게 번호 ' + phone_num)
        print("-"*50)
    
    switch_left()
        
    loop = False

```


