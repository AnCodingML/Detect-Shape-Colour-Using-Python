import cv2
import time
import numpy as np
cap = cv2.VideoCapture(1)
img = cap.set(cv2.CAP_PROP_FRAME_WIDTH, 480)
img = cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 360)
#sesuaikan data dengan tabel 26.2
#lower_range = np.array ([1_h,1_s,1_v])
low_range = np.array([46, 46, 153], np.uint8) #data 1

#upper_range = np.array ([u_h,u_s,u_v])
high_range = np.array([180, 255, 255], np.uint8) #data 2

fpsLimit = 1 #batas throttle
startTime = time.time()
while True:
    _,img = cap.read()
    img = cv2.flip(img, 1) #untuk membalikkan kamera yang terbalik
    nowTime = time.time()
    if (int(nowTime - startTime)) > fpsLimit:
        #lakukan hal-hal cv2 lainnya...
        startTime = time.time() #setel ulang waktu

    hue_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    threshold_img = cv2. inRange (hue_img, low_range, high_range)
    contours, hierarchy = cv2.findContours (threshold_img,
                                            cv2. RETR_CCOMP,
                                            cv2.CHAIN_APPROX_SIMPLE)
    for cnt in contours:
        area = cv2.contourArea(cnt)

        if area > 400:
            (x, y, w, h) = cv2.boundingRect(cnt)
            xx = int (x + (x+w)/2)
            yy = int (y + (y+h)/2)
            output = "X{0:.0f}Y{1:.0f}Z".format(xx, yy)
            print("output = '" + output + "'")

            if cv2.contourArea(cnt) < 700:
                continue

            imageFrame = cv2.rectangle(img, (x, y),
                                       (x + w, y + h),
                                       (0, 0, 255), 2)

        cv2.imshow("Color Detection", img)

        if cv2.waitKey(1) &0xFF == ord('q'):
            break

cap.release()
cv2.destroyAllWindows()
