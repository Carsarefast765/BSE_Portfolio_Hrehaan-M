# Ball Tracking Robot
My project the ball tracking robot that uses a raspberry pi connected to a pi camera to track a red ball. It uses image processing to make all of the red pixels of the ball on the screen white and all of the non-red pixels black. If it sees the ball on the left or right side of the screen it turns to face it then goes towards it. I also added a servo mount to the Pi cam as a modification as well as some LEDs one to iluminate what the camera is seeing and the other to blink when the code is running as a saftey feature.


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Hrehaan M | Skyline High School | Mechanical Engineering | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone



<iframe width="560" height="315" src="https://www.youtube.com/embed/Q3FEVGCqfsc?si=u8i5lzmaQo-Skwz2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


Since my last milestone I completed the my base project. I got the image proccessing working so the robot only sees the ball. I the ball is in the center of the screen it goes forward and if it is on the left or right it turns so it is in the center then goes forward. It also checks if the ball takes up most of the screen if it does then it stops. Then if the ultrasonic sensor detects an object it stops. If the car does not see the ball it starts to spin right slowly until it detects the ball. When I was writing the code for this project there were many errors and problems but the biggest one happened after everything started working. The next day it all sudenly stoped working and it was even more frustrating because everything had been working. I eventualy figured out that some of the ultrasonic sensor wires got randomly got unplugged so the code was getting stuck. Next I plan to add my modifications since my base project is complete. I plan on adding a gimbel for the raspberry pi so it can turn to the left right up and down. I also plan on adding LED lights to the front to improve the image that the pi cam is getting and an LED light that turns on only when the program is running so people know not to go near it when is is running.

# First Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/yT9xCVv3_c0?si=ECK1BJt1ItTgbwod" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>



My project is the ball tracking robot which uses ultrasonic sensors and a raspberry pi camera to track a red ball. The camera an ultrasonic sensors are all connected to a raspberry pi 4. For my first milestone I finished building the base of my car with the motors and got them connected to the raspberry pi through an H bridge. After that i wrote code to test if the motors worked and the car could move forward, backwars, right, and left. Then i wired up my Ultrasonic sensors to the raspberry pi and wrote code to test them to make sure they worked. Lastly I wrote code to combine the movement of the motors with the what the ultrasonic sensor detects so if the left ultrasonic sensor detects an object less than 5 cm away the car turns to the left if the right ultrasonic sensor detects an object less than 5 cm away the car turns to the right and if the middle one detects an object less than 5 cm away it stops. For my next milestone my plan is to attach the pi camaera to the car and get it to be able to detect the ball.


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
import RPi.GPIO as GPIO
import time
import cv2 #OpenCV
from picamera2 import Picamera2, Preview
import numpy as np








GPIO.setmode(GPIO.BCM)
GPIO.setup(17, GPIO.OUT)
GPIO.setup(22, GPIO.OUT)
GPIO.setup(23, GPIO.OUT)
GPIO.setup(24, GPIO.OUT)
GPIO.setup(14, GPIO.OUT)
GPIO.setup(15, GPIO.OUT)
GPIO.setup(4,GPIO.OUT)
bottom = GPIO.PWM(4,50)
GPIO.setup(3,GPIO.OUT)
top = GPIO.PWM(3,50)

def right(sec):
   GPIO.output(17, True)
   GPIO.output(22, False)
   GPIO.output(23, True)
   GPIO.output(24, False)
   time.sleep(sec)
   stop()


def left(sec):
   GPIO.output(17, False)
   GPIO.output(22, True)
   GPIO.output(23, False)
   GPIO.output(24, True)
   time.sleep(sec)
   stop()


def stop():
   GPIO.output(17, False)
   GPIO.output(22, False)
   GPIO.output(23, False)
   GPIO.output(24, False)


def reverse(sec):
   GPIO.output(17, True)
   GPIO.output(22, False)
   GPIO.output(23, False)
   GPIO.output(24, True)
   time.sleep(sec)
   stop()


def forward():
   GPIO.output(17, False)
   GPIO.output(22, True)
   GPIO.output(23, True)
   GPIO.output(24, False)
   #time.sleep(sec)
   #stop()



def top_set_angle(angle):
    duty = angle / 18.0 + 2.5
    GPIO.output(3, True)
    top.ChangeDutyCycle(duty)
    time.sleep(0.5)
    GPIO.output(3, False)
    top.ChangeDutyCycle(0)
def light_blink(LED_PIN):
    GPIO.output(LED_PIN, GPIO.HIGH)
    time.sleep(1)
    GPIO.output(LED_PIN, GPIO.LOW)
    GPIO.cleanup()
def bottom_set_angle(angle):
    duty = angle / 18.0 + 2.5
    GPIO.output(4, True)
    bottom.ChangeDutyCycle(duty)
    time.sleep(1)
    GPIO.output(4, False)
    bottom.ChangeDutyCycle(0)


# GPIO pins setup for Ultrasonic Sensor 1
TRIG1 = 16  # GPIO 16 (pin 36)
ECHO1 = 26  # GPIO 26 (pin 37)


# GPIO pins setup for Ultrasonic Sensor 2
TRIG2 = 27  # GPIO 27 (pin 13)
ECHO2 = 25  # GPIO 25 (pin 22)


# GPIO pins setup for Ultrasonic Sensor 3
TRIG3 = 6   # GPIO 6  (pin 31)
ECHO3 = 5   # GPIO 5  (pin 29)


def setup():
   GPIO.setmode(GPIO.BCM)


   # Setup for Ultrasonic Sensor 1
   GPIO.setup(TRIG1, GPIO.OUT)
   GPIO.setup(ECHO1, GPIO.IN)


   # Setup for Ultrasonic Sensor 2
   GPIO.setup(TRIG2, GPIO.OUT)
   GPIO.setup(ECHO2, GPIO.IN)


   # Setup for Ultrasonic Sensor 3
   GPIO.setup(TRIG3, GPIO.OUT)
   GPIO.setup(ECHO3, GPIO.IN)


def distance(trig, echo):
   # Ensure the trigger pin is low initially
   GPIO.output(trig, False)
   time.sleep(0.1)


   # Generate a short pulse to trigger the sensor
   GPIO.output(trig, True)
   time.sleep(0.00001)
   GPIO.output(trig, False)


   # Measure the duration of the echo pulse
   while GPIO.input(echo) == 0:
       pulse_start = time.time()


   while GPIO.input(echo) == 1:
       pulse_end = time.time()


   pulse_duration = pulse_end - pulse_start


   # Speed of sound at sea level is 343m/s, or 34300 cm/s
   # Divide by 2 because we're measuring to and from the object
   distance = (pulse_duration * 34300) / 2


   return distance
def segment_colour(frame):  
   hsv_roi =  cv2.cvtColor(frame, cv2.COLOR_RGB2HSV)
  
   mask_1 = cv2.inRange(hsv_roi, np.array([130, 150,40]), np.array([190,255,255]))


   mask = mask_1
   kern_dilate = np.ones((12,12),np.uint8)
   kern_erode  = np.ones((6,6),np.uint8)
   mask= cv2.erode(mask,kern_erode)     #Eroding
   mask= cv2.dilate(mask,kern_dilate)     #Dilating
  
   (h,w) = mask.shape
   cv2.namedWindow('mask',cv2.WINDOW_NORMAL)
   cv2.resizeWindow('mask',800,600)
   cv2.imshow('mask', mask) # Shows mask (B&W screen with identified red pixels)
  
   return mask


def find_blob(blob): 
   largest_contour=0
   cont_index=0
   contours, hierarchy = cv2.findContours(blob, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_SIMPLE)
   for idx, contour in enumerate(contours):
       area=cv2.contourArea(contour)
       if (area >largest_contour):
           largest_contour=area
           cont_index=idx
                  
   r=(0,0,2,2)
   if len(contours) > 0:
       r = cv2.boundingRect(contours[cont_index])
   
   return r,largest_contour
def get_centerX(x,w):
   centerX = (w/2) + x
   return centerX
def get_centerY(y,h):
   centerY = (h/2) + y
   return centerY
def draw_center(x,center_x,center_y):
   cv2.circle(x,center_x,center_y,10,(255,0,0),-1)

if __name__ == '__main__':
   try:
        picam2 = Picamera2()


        camera_config = picam2.create_still_configuration(main={"size": (1920, 1080)}, lores={"size": (480, 270)}, display="lores")




        picam2.configure(camera_config)
        picam2.start_preview(Preview.QTGL) 
        picam2.start()

        
        time.sleep(2)
        top.start(0)
        bottom.start(0)
        angle = 80
        top_set_angle(angle)
        GPIO.output(14,GPIO.HIGH)
        GPIO.output(15,GPIO.HIGH)
        while(True):


           im = picam2.capture_array()
           height = im.shape[0]
           width = im.shape[1]


           setup()

           hsv1 = cv2.cvtColor(im, cv2.COLOR_RGB2HSV)
        #fucntion 1
           mask_red=segment_colour(im[:,:,[0,1,2]])
        #fucntion 2
           #time.sleep(0.1)

           loct,area=find_blob(mask_red)
          
           x,y,w,h=loct
           #print(area)
           centerx = get_centerX(x,w)
           centery = get_centerY(y,h)
           #print(centery)
           #print("x = " + str(get_centerX(x,w)))
           #print("y = " + str(get_centerY(y,h)))
           #print(top_duty)
           print(angle)
           if(centery > 700 and angle < 175):
               #top_duty += 0.5
               #top.start(0)
               angle -= 3
               top_set_angle(angle)
               #time.sleep(0.5)
               #top.stop()
           elif(1 < centery < 300 and angle > 5):
               #top_duty += 0.5
               #top.start(0)
               angle += 3
               top_set_angle(angle)
               #time.sleep()
           if(centerx > 1200):
               right(0.1)
           elif(centerx < 600):
               left(0.1)
           else:
               if(centerx > 500 and centerx < 1300 and area < 1000000):
                   forward()
               else:
                   stop()
               '''
               dist1 = distance(TRIG1, ECHO1)
                   #print(f"Distance from Sensor 1: {dist1:.1f} cm")
                  
                  
                   # Measure distance for Ultrasonic Sensor 2
               dist2 = distance(TRIG2, ECHO2)
                   #print(f"Distance from Sensor 2: {dist2:.1f} cm")
                  
                  
                   # Measure distance for Ultrasonic Sensor 3
               dist3 = distance(TRIG3, ECHO3)
                   #print(f"Distance from Sensor 3: {dist3:.1f} cm")
               if(dist1 <= 5):
                   right(0.2)
               elif(dist2 <= 5):
                   left(0.2)
               elif(dist3 <= 2):
                   stop()

               '''
               
           #print()
           #cv2.circle(im,(get_centerX(x,w),get_centerY(y,h)),100,(255,0,0),-1)
           #cv2.namedWindow('x',cv2.WINDOW_NORMAL)
           #cv2.resizeWindow('x',800,600)
          
           #cv2.imshow('x',loct)



   except KeyboardInterrupt:
       stop()
       GPIO.cleanup()

       top.stop()





# Measure distance for Ultrasonic Sensor 1
          





```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi Kit | controls everything | 95.91 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1"> Link </a> |
| Robot Chassis | base of the car | $18.99 | <a href="https://www.amazon.com/Smart-Chassis-Motors-Encoder-Battery/dp/B01LXY7CM3/ref=sr_1_5?crid=373Y5YK6JWMD&keywords=robot+chassis&qid=1687740144&sprefix=robot+chassi%2Caps%2C93&sr=8-5"> Link </a> |
| Screwdriver Kit | What the item is used for | $5.94 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |
| Ultrasonic Sensor | detect how close an object is to the car | $9.99 | <a href="https://www.amazon.com/WWZMDiB-HC-SR04-Ultrasonic-Distance-Measuring/dp/B0CQCCGXCP/ref=sr_1_1_sspa?crid=3J2JR973WKPHO&dib=eyJ2IjoiMSJ9.E2SIkElJhtFWCJCHL5Q6Y73Ys_HCMPRVFCIrG_zKv4Og7BdZNtr69Mkju140lhlfzFGQuY542jpsp8FMrtV9d2hCBI7D8lYTH9bcgDXZhs4941uj-d1D69ZYdKmAI1Jig3VmYXOl3axVQ8Jq5L3nGRymNMtNbxkaFqGNyzkq4p37hhxU6jheuoaMo3Onz2FE9ILThkjUbdxRNW3rrZgZ7bYj9mf-yav85hBAmNduYyo.EneY3GmHDfDjDwhdUdDQ4Ktk6fECH62Adb42cEkehRc&dib_tag=se&keywords=ultrasonic%2Bsensor&qid=1715961326&sprefix=ultrasonic%2Bsensor%2Caps%2C72&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| H Bridge | control the motors | $7.79 | <a href="https://www.amazon.com/HiLetgo-H-bridge-Stepper-Controller-Arduino/dp/B00M0F243E/ref=pd_lpo_sccl_1/142-4739935-9789822?pd_rd_w=5sLcA&content-id=amzn1.sym.4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_p=4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_r=KR0X36CCWH30E7T108Y9&pd_rd_wg=1cZ0l&pd_rd_r=420e26d6-ea71-4fc7-adcf-6cf8ffb38079&pd_rd_i=B00M0F243E&psc=1"> Link </a> |
| Pi Cam | record a live feed to see the ball | $12.86 | <a href="https://www.amazon.com/gp/product/B07RWCGX5K/ref=ox_sc_act_title_1?smid=A2IAB2RW3LLT8D&psc=1"> Link </a> |
| Electronics Kit | wire ultrasonic sensors LEDs and H-bridge | $11.98 | <a href="https://www.amazon.com/EL-CK-002-Electronic-Breadboard-Capacitor-Potentiometer/dp/B01ERP6WL4/ref=sr_1_4?crid=30T5LTYVQLQ7Z&dib=eyJ2IjoiMSJ9.XZtpck6Llt4UIuYeKM4X3BoXzDuzolZMTCtFDj-oTh1vuIi0HYJZJEdpS-MCdGCK1AWUbUmgoEswoRPxGUSKeGRTzsciRE_l2Vrp8FGX1SxK-HmibPNyHBEtkFJKo_OYmMhkhdCJ4OIH38ALRfFvrXZ7OU5faZVvkTBqod8p7UZYwNwdLCcimwFWGWKaDa-gbbx_TGk7lYQmEbrzeL4UXM-gW3RDtuOV0dCykxwyvYJKCCcOhrK3f18N4NZjiqL_Y5noE1rQTmwyFcG67DzgpNaUPanwIQaYfCe5mgD-njY.v6mU1wYX4M5ShCiyrZMey0hbOwvqLszD8axpHbKlA6I&dib_tag=se&keywords=mini+breadboard+kit&qid=1716419767&s=electronics&sprefix=mini+breadboard+kit%2Celectronics%2C106&sr=1-4"> Link </a> |
| Motors | move the car | $11.98 | <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_4?crid=1JP29NIWBLH2M&dib=eyJ2IjoiMSJ9.Wq3jKgOLbqtEP772vMD4pV5f-w3PLBdEpKqguykXOb0JFO14f4Dq0m_VDVUMUFtR8WFINUEticI3GXcoGqwXPqK9yIh04PhCktgccMz9zAUiKXMJPwmOTUp_6av3XuFD0lXo9WngN9iKI6YgZrhEEs9qnqbcB1GnvgntCdKz8Q1dFuNu61NgSE6Z8vBk3FRpaNcr1lCI7FApTiNi0Qce8gbfmMn6oUggZQHpIOKKZ6s.M7WsZ_ZZtm3rm93kKgw0NOxt1McVBYX6m55oGxu1xxI&dib_tag=se&keywords=dc+motor+with+gearbox&qid=1715911706&sprefix=dc+motor+with+gearbox%2Caps%2C126&sr=8-4"> Link </a> |
| DMM | check if there is a wiring problem | $11 | <a href="https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital+multimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital+multimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1"> Link </a> |
| AA batteries | power the motors and ultrasonic sensors | $14.89 | <a href="https://www.amazon.com/Energizer-Batteries-Double-Alkaline-Battery/dp/B07TXNX6S2/ref=sr_1_8?crid=2Z92KWYJCEV4C&dib=eyJ2IjoiMSJ9.dtP_LjZ3n55Hl_vfONYcp2Yt-k3sdCQ3aoBRCYeyTDCJ82ozrmrPxo-6Tr4waSviK9o963tO4XU9etNcf1xOPvNGHLDY4bsnRPXSlry5pzg1f8E4bWO0BYSU8hIMC5dsOpkzclVaZgrmIbW8NZgWtTxSa7_pA4e7DDVE7eEQ0ET5hocBS4_Y5e755cH3YxE9_mAvjujhceYX3ReQamwRcgHX8QK-DhZuspvK1OU9KUA.vnGDnHsJH3R8dzynuZmiz-qIPu2mUbpk_jFQHsW0xb8&dib_tag=se&keywords=AA+batteries&qid=1715957810&refinements=p_85%3A2470955011&rnid=2470954011&rps=1&sprefix=aa+batterie%2Caps%2C88&sr=8-8"> Link </a> |
| Champion sports ball | item for robot to track | $12.34 | <a href="https://www.amazon.com/Champion-Sports-Coated-Density-8-5-Inch/dp/B000KYQ410/ref=pd_lpo_sccl_1/142-4739935-9789822?pd_rd_w=uDrsq&content-id=amzn1.sym.4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_p=4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_r=0VXWNQB5508GXN7H55RW&pd_rd_wg=8uq7L&pd_rd_r=aa130b62-c739-4591-b544-8b45dc9a1d92&pd_rd_i=B0CN8XJZ23&th=1&psc=1"> Link </a> |
| USB power bank | power the raspberry pi | $16.91 | <a href="https://www.amazon.com/SIXTHGU-Portable-Charger-Charging-Flashlight/dp/B0C7PHKKNK/ref=sr_1_2_sspa?crid=2ZZM4AAZMMWHQ&dib=eyJ2IjoiMSJ9.W2Zx5_I3mKOn6UpwAzOw6PD0PNh1iaMRBiedequdv9weeWL0HPyPcxJBR9h6-LiFW-sHKnHSApN0sUxx0Q9xIRs80R57IlvvCsmEzXcktogo-4nP-NxrEZOy5dJTcXY8N-PBwfGt4fl_9LP8npenzDUV9TPA8KN6DMu175g6JegC_gZhAJrbqX94EfpQhLwP9vIJH45w2N-AFrfZZOy9jqk55gzVyk4Qst8uZvqn768.KBrc5_SqZ4e8zCpoFc-1C7rk02t3o2ykgDPB65W5JJU&dib_tag=se&keywords=always%2Bon%2Bpower%2Bbank&qid=1715957917&sprefix=always%2Bon%2Bpower%2Bbank%2Caps%2C107&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Camera tilt platform | move the pi cam up down left and right | $26.99 | <a href="https://www.amazon.com/Arducam-Upgraded-Camera-Platform-Raspberry/dp/B08PK9N9T4/ref=sr_1_5?dib=eyJ2IjoiMSJ9.uf64hZIk7fZT-s5oIshEmaoCWR0e1G9NOSzPRro6Qk8GBmqcQB0QolfaBj40qt2zh3JK_I8OitrjCrDqpMsl2W4sdej2X1ZQ6lJYQVOmm4lEE6beNjfTgKc558djYXWzadOT5Ui-Rbhuw8I0AwXF48uPFSYPMhMxAyFYAOLHRjfDPQX06ik31z4PwyhV3K61sH68YA3R_FVZiqa-5jtXsKugmPSt5rKSgb2Kbw7jfxI.aiv2yJcOb5lKM_9pOWbBspy0hKvGvedxbXCXxpfKT8c&dib_tag=se&keywords=pi+camera+mount&qid=1720706899&sr=8-5"> Link </a> |



# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
