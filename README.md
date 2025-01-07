# eye-controlled-mouse
#kumar-Gaurav
Operate Computer with Eyes
import cv2
import mediapipe as mp
import pyautogui
face_mesh = mp.solutions.face_mesh.FaceMesh(refine_landmarks=True)
cam = cv2.VideoCapture(0)
screen_w , screen_h = pyautogui.size()
while True:
    _, frame = cam.read()
    frame = cv2.flip(frame, 1)
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output = face_mesh.process(rgb_frame)
    landmark_points = output.multi_face_landmarks
    frame_h , frame_w , _ = frame.shape
    if landmark_points:
        landmarks = landmark_points[0].landmark
        for id, landmark in enumerate(landmarks[474:478]):
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 0))
            if id == 1:
                screen_x = screen_w/ frame_w * x
                screen_y = screen_h / frame_h * y
                pyautogui.moveTo(screen_x, screen_y)
        left = [landmarks[145], landmarks[159]]
        for landmark in left:
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 255))
        if (left[0].y - left[1].y) < 0.004:
            pyautogui.click()
            pyautogui.sleep(1)
        right = [landmarks[374], landmarks[386]]  # Right eye upper and lower eyelids
        for landmark in right:
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (255, 0, 0))  # Draw circle for right eye landmarks

        # Right Eye Blink Detection (distance between upper and lower eyelid)
        if (right[0].y - right[1].y) < 0.004:  # Blink detection for right eye
            pyautogui.click()
            pyautogui.sleep(1)           
#cv2-for-cmputer-vision
#pyautogui-for-operating-cursor
#mediapipe for facial landmarks detection

    cv2.imshow('Eye Controlled Mouse', frame)
    cv2.waitKey(1)
    #eye-controlled-mouse
