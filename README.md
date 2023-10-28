# Color_recognization
Place the object in front of the camera and it will detect the color of the object.


    import cv2
    import pandas as pd
    cap=cv2.VideoCapture(0)
    cap.set(cv2.CAP_PROP_FRAME_WIDTH,1288)
    cap.set(cv2.CAP_PROP_FRAME_WIDTH,728)
    index = ["color", "color_name", "hex", "R", "G", "B"]
    csv = pd.read_csv('colors.csv', names=index, header=None)
    def get_color_name(R, G, B):
        minimum = 10000
        for i in range(len(csv)):
            d = abs(R - int(csv.loc[i, "R"])) + abs(G - int(csv.loc[i, "G"])) + abs(B - int(csv.loc[i, "B"]))
            if d <= minimum:
                minimum = d
                cname = csv.loc[i, "color_name"]
        return cname
    while True:
        _, frame=cap.read()
        hsv_frame = cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)
        height, width, _ = frame.shape
    
        cx=int(width/2)
        cy=int(height/2)
    
        # Pick pixel value
        pixel_center=frame[cy,cx]
        hue_value=pixel_center[0]
        
        color=get_color_name(pixel_center[2],pixel_center[1],pixel_center[0])
        
        pixel_center_bgr=frame[cy,cx]
        b,g,r=int(pixel_center[0]),int(pixel_center_bgr[1]),int(pixel_center_bgr[2])
        cv2.putText(frame,color,(10,70),0,1.5,(b,g,r),2)
        cv2.circle(frame,(cx,cy),5,(25,25,25),3)
    
        cv2.imshow("Frame",frame)
        key=cv2.waitKey()
        if key==27:
            break
    cap.release()
    cv2.destroyAllWindows()
