import cv2
import numpy as np
from time import sleep

width_min = 80 #Minimum rectangle width
height_min = 80 #Minimum rectangle hight

offset = 6 #Allowed error between pixels

pos_line = 550 # Position of the counting line

delay = 60 #Video FPS

detect = []
cars = 0

def get_center (x, y, w, h):
     x1 = int (w / 2)
     y1 = int (h / 2)
     cx = x + x1
     cy = y + y1
     return cx, cy
    
cap = cv2.VideoCapture ('C:/Users/lenovo/Downloads/video.mp4')
subtraction = cv2.bgsegm.createBackgroundSubtractorMOG ()
while True:
    ret, frame = cap.read ()
      time = float (1 / delay)
      sleep (time)
      gray = cv2.cvtColor (frame, cv2.COLOR_BGR2GRAY)
      blur = cv2.GaussianBlur (gray, (3,3), 5)
      img_sub = subtraction.apply (blur)
      dilat = cv2.dilate (img_sub, np.ones ((5,5)))
      kernel = cv2.getStructuringElement (cv2.MORPH_ELLIPSE, (5, 5))
      dilated = cv2.morphologyEx (dilat, cv2. MORPH_CLOSE, kernel)
      dilated = cv2.morphologyEx (dilated, cv2. MORPH_CLOSE, kernel)
      contour, h = cv2.findContours (expanded, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
  cv2.line (frame, (25, pos_line), (1200, pos_line), (255,127,0), 3)
      for (i, c) in enumerate (outline):
          (x, y, w, h) = cv2.boundingRect (c)
          validate_outline = (w> = width_min) and (h> = height_min)
          if not validate_outline:
              continues
         cv2.rectangle (frame, (x, y), (x + w, y + h), (0,255,0), 2)
          center = center_pick (x, y, w, h)
          detec.append (center)
          cv2.circle (frame, center, 4, (0, 0.255), -1)
         for (x, y) in detec:
              if y <(pos_line + offset) and y> (pos_line-offset):
                  cars + = 1
                  cv2.line (frame, (25, pos_line), (1200, pos_line), (0,127,255), 3)
                  detec.remove ((x, y))
                  print ("car is detected:" + str (cars))
       
   cv2.putText (frame, "VEHICLE COUNT:" + str (cars), (450, 70), cv2.FONT_HERSHEY_SIMPLEX, 2, (0, 0, 255), 5)
      cv2.imshow ("Video Original", frame)
      cv2.imshow ("Detect", extended)
     if cv2.waitKey (1) == 27:
          break
    
cv2.destroyAllWindows ()
cap.release ()
