import cv2
import time
import numpy as np

cap = cv2.VideoCapture(1)
img = cap.set(cv2.CAP_PROP_FRAME_WIDTH,480)
img = cap.set(cv2.CAP_PROP_FRAME_HEIGHT,360)

#lower_range = np.array([l_h,l_s,l_v])
low_range = np.array([46, 46, 153],np.uint8)    #data 1
#low_range = np.array([110, 153, 47],np.uint8)

#upper_range = np.array([u_h,u_s,u_v])
high_range = np.array([180, 255, 255],np.uint8)  #data 1
#high_range = np.array([180, 255, 204],np.uint8)

fpsLimit = 1    #throttle limit
startTime = time.time()

while True:
    _, img = cap.read()

    img = cv2.flip(img,1)   #untuk membalikkan kamera yang terbalik

    nowTime = time.time()
    if (int(nowTime - startTime)) > fpsLimit:
        #do other cv2 stuff....
        startTime = time.time() #reset time

    hue_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    threshold_img = cv2.inRange(hue_img, low_range, high_range)

    contours, hierarchy = cv2.findContours(threshold_img,
                                           cv2.RETR_CCOMP,
                                           cv2.CHAIN_APPROX_SIMPLE)
    for cnt in contours:
        area = cv2.contourArea(cnt)
        approx = cv2.approxPolyDP(cnt,0.01*cv2.arcLength(cnt,True),True)

        x = approx.ravel()[0]
        y = approx.ravel()[1]

        if area > 400:
            cv2.drawContours(img, [approx], 0, (0, 0, 255), 2)
            (x, y, w, h) = cv2.boundingRect(cnt)
            
            if cv2.contourArea(cnt) < 700:
                continue

            if len(approx) == 3:
                print("Segitiga")
                cv2.putText(img, "segitiga", (x, y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0,0), 2)
            elif len(approx) == 4:
                x, y, w, h = cv2.boundingRect(approx)
                ratio = w / float(h)
                area=w*h
                #shape = "Square" if ratio >= 0.95 and ratio 1.05 else "Rectangle"
                if ratio >= 0.95 and ratio <= 1.05:
                    print("Persegi")
                    cv2.putText(img, "Persegi",(x,y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2)

                else:
                    print("Persegi")
                    cv2.putText(img, "Persegi Panjang",(x,y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2)
                    
            elif len(approx) ==5:
                print("Segilima")
                cv2.putText(img, "Segilima",(x, y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2)

            elif len(approx) ==6:
                print("Segienam")
                cv2.putText(img, "Segienam",(x, y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2)
                
            elif 10 < len(approx) < 20:
                print("Lingkaran")
                cv2.putText(img, "Lingkaran",(x, y), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2)

    cv2.imshow("Shape Tracking", img)

    if cv2.waitKey(1) &0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
