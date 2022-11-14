# README

## About

Go Wails를 사용하여 만든 Image Resizer입니다.

이미지 배율이 1:1이여야 만 올라가는 곳이 존재하는데, 그럴때 사용하기 위해 만든 유틸입니다.

## 기능
- 이미지를 불러와 1:1 비율로 자동으로 resize 하여 저장
- 저장시 PNG, JPG 둘중 하나로 저장 가능(기본 PNG)
- 이미지의 크기를 수동으로 지정 가능(기본 1:1 - 가장 긴 크기를 동일하게 늘림)

## 빌드
```
cd go-imgresizer
wails build
```