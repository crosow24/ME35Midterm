import requests
key = "REDACTED"
headers = {'Authorization': 'Bearer ' + str(key),'Content-Type':'application/json'}

cam.snap()
cv2_image = cv2.cvtColor(np.array(cam.raw_image), cv2.COLOR_RGB2BGR)
b,g,r = cv2.split(cv2_image)
grey = cv2.cvtColor(cv2_image, cv2.COLOR_BGRA2GRAY)
cam.show(grey)  # shows any cv2 image in the same spot on the webpage (third image)
image3 = Image.fromarray(grey)
hsvRed = cv2.cvtColor(cv2_image, cv2.COLOR_BGR2HSV)
hsvBlue = cv2.cvtColor(cv2_image, cv2.COLOR_BGR2HSV)
result = hsvRed.copy()
resultBlue = hsvBlue.copy()
lower_red1 = np.array([0, 100, 20])
upper_red1 = np.array([10, 255, 255])
lower_red2 = np.array([160,100,20])
upper_red2 = np.array([179,255,255])
lower_blue = np.array([110,50,50])
upper_blue = np.array([130,255,255])

lower_red_mask = cv2.inRange(hsvRed, lower_red1, upper_red1)
upper_red_mask = cv2.inRange(hsvRed, lower_red2, upper_red2)
blue_mask = cv2.inRange(hsvRed, lower_blue, upper_blue)

full_red_mask = lower_red_mask + upper_red_mask;
 
result = cv2.bitwise_and(result, result, mask=full_red_mask)
resultBlue = cv2.bitwise_and(result, result, mask=blue_mask)

cam.show(full_red_mask)
cam.show(blue_mask)
redMask = Image.fromarray(full_red_mask)
blueMask = Image.fromarray(blue_mask)
display(redMask)
display(blueMask)
redMaskHist = cv2.cvtColor(np.array(redMask), cv2.COLOR_RGB2BGR)
blueMaskHist = cv2.cvtColor(np.array(redMask), cv2.COLOR_RGB2BGR)


#cv2.imshow('mask', full_red_mask)
#cv2.imshow('result', result)

redVal=np.sum(redMaskHist)
blueVal=np.sum(blueMaskHist)
textBox.innerText=repr(np.sum(redMaskHist))
if redVal>10000000:
    json_data = {
    'records': [
        {
            'id': 'recePqOkNgy5XjLkd',
            'fields': {
                'Name': 'Current Color',
                'Status': 'Red',
                'Unit': 'Item 1',
            },
        },
    ],
    }
    response = requests.patch('REDACTED', headers=headers, json=json_data)
    print(response)
if blueVal>1000000:
    json_data = {
    'records': [
        {
            'id': 'recePqOkNgy5XjLkd',
            'fields': {
                'Name': 'Current Color',
                'Status': 'Blue',
                'Unit': 'Item 1',
            },
        },
    ],
    }
    response = requests.patch('REDACTED', headers=headers, json=json_data)
    print(response)
