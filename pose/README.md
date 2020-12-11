### OpenPose 다운받기
OpenPose : Caffe와 OpenCV를 기반으로 구성된 손, 얼굴 포함 몸의 움직임을 추적해주는 API

1. 아래의 깃허브에서 오픈 포즈 다운로드<br>
https://github.com/CMU-Perceptual-Computing-Lab/openpose

2. getModels로 모델을 다운로드
..\openpose-master\models\pose\coco 폴더의 COCO모델 사용
- pose_deploy_linevec.prototxt
- pose_iter_440000.caffemodel<br>

### COCO모델에서 제공하는 POINT
COCO Output Format Nose – 0, Neck – 1, Right Shoulder – 2, Right Elbow – 3, Right Wrist – 4, Left Shoulder – 5, Left Elbow – 6, Left Wrist – 7, Right Hip – 8, Right Knee – 9, Right Ankle – 10, Left Hip – 11, Left Knee – 12, LAnkle – 13, Right Eye – 14, Left Eye – 15, Right Ear – 16, Left Ear – 17, Background – 18<br>
우리 프로젝트에서 상체의 움직임중 끄덕임등 목과 머리의 움직임을 파악하기 위해서 Nose, Neck, Right Eye, Left Eye, Right Ear, Left Ear 사용.<br>
Pose pair중 필요한 상체 연결만 남김.<br>
POSE_PAIRS = [[1,0],[1,2],[1,5],[2,3],[3,4],[5,6],[6,7],[0,14],[0,15],[14,16],[15,17]]
<div>
  <img width="259" alt="화면 캡처 2020-12-10 205622" src="https://user-images.githubusercontent.com/63234878/101769370-35ab0e00-3b2a-11eb-85e2-72b065f7d5ee.png">
</div><br>

### 끄덕임 검출
목과 코가 둘다 검출 되었을때 그 길이를 계산하여 끄덕임을 검출 
  
      if points[0] and points[1]:
        d.append(points[0][0]-points[1][0])
        d.append(points[0][1]-points[1][1])
        neck = math.sqrt(d[0]*d[0]+d[1]*d[1])
        print(neck)
    if neck > 200:
        cv2.putText(frame, "head up", (50, 100), cv2.FONT_HERSHEY_COMPLEX, .6, (255, 50, 0), 2, lineType=cv2.LINE_AA)
    if neck < 140:
        cv2.putText(frame, "head down", (50, 100), cv2.FONT_HERSHEY_COMPLEX, .6, (255, 50, 0), 2, lineType=cv2.LINE_AA)
    cv2.putText(frame, "time taken = {:.2f} sec".format(time.time() - t), (50, 50), cv2.FONT_HERSHEY_COMPLEX, .6, (255, 50, 0), 2, lineType=cv2.LINE_AA)
    cv2.imshow('Output-Skeleton', frame)

### reference
https://www.learnopencv.com/deep-learning-based-human-pose-estimation-using-opencv-cpp-python/ <br>
https://github.com/natanielruiz/deep-head-pose


