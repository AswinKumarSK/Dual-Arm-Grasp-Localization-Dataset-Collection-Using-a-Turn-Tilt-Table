# Design and Implementation of Turn-Tilt Table For Automated Dual Arm Grasp Localization Dataset collection

##### This project aims at developing a turn-tilt table for capturing the human fingers’ (index and thumb) pose automatically by integrating multi-sensory data to prepare a dataset containing annotations required for a deep learning approach in a dual arm manipulator-based grasp localization scenario. The dataset preparation is aimed to be carried out in a human imitated approach by allowing different people to attempt to grasp objects with two fingers and two hands.

##### In the case of a 3PRS (3-Revolute-Prismatic-Spherical) mechanism, kinematics refers to the study of the motion and position of the mechanism's end effector.The kinematics of the 3PRS mechanism in this project were determined using the vector loop equation method mentioned below:

![Vector Loop Diagram 2D](https://github.com/AswinKumarSK/Dual-Arm-Grasp-Localization-Dataset-Collection-Using-a-Turn-Tilt-Table/assets/74086002/39f76e62-9cb5-420e-b339-e9c3117ec9bb)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Vector Loop Diagram 3D](https://github.com/AswinKumarSK/Dual-Arm-Grasp-Localization-Dataset-Collection-Using-a-Turn-Tilt-Table/assets/74086002/8ca785d8-cc3e-4a92-80c7-1d2d28c6bfe7)

##### The Code for Kinematics is given below:
```
import math
# create a 3x3 matrix
matrix1 = [[0 for i in range(3)] for j in range(3)]
matrix2 = [[0 for i in range(3)] for j in range(3)]
matrix3 = [[0 for i in range(3)] for j in range(3)]
dia = [[0] * 1 for j in range(3)] 
base = [[0] * 1 for j in range(3)] 
centre = [[0] * 1 for j in range(3)] 
link = [[0] * 1 for j in range(3)] 
a=float(input("Enter value for x in degrees: "))
b=float(input("Enter value for y in degrees: "))
c=float(input("Enter value for z in degrees: "))
diax=float(input("Enter x value of the dia:"))
diay=float(input("Enter y value of the dia:"))
b1=float(input("Enter x length of base link:"))
b2=float(input("Enter y length of base link:"))
b3=float(input("Enter z length of base link:"))
l1=float(input("Enter x length of link:"))
l2=float(input("Enter y length of link:"))
l3=float(input("Enter z length of link:"))
centrez=float(input("Enter the length of centre to top:"))
# take input values from user
for i in range(3):
    for j in range(3):
        if ((i == 0 and j == 1) or (i == 0 and j == 2) or (i == 1 and j == 0) or (i == 2 and j == 0)):
            matrix1[i][j] = 0
        elif i == 0 and j == 0:
            matrix1[i][j] = 1
        elif (i==1 and j==1) or (i==2 and j==2):
            user_input = a
            angle = float(user_input)
            matrix1[i][j] = math.cos(angle)
        elif(i==1 and j==2):
            user_input = a
            angle = float(user_input)
            matrix1[i][j] = -(math.sin(angle))
        else:
            user_input = a
            angle = float(user_input)
            matrix1[i][j] = math.sin(angle)
for i in range(3):
    for j in range(3):
        if ((i==0 and j==1 ) or (i==1 and j==0) or (i==1 and j==2) or (i==2 and j==1)):
            matrix2[i][j]=0
        elif(i==1 and j==1):
            matrix2[i][j]=1
        elif(i==0 and j==0) or (i==2 and j==2):
            user_input = b
            angle = float(user_input)
            matrix2[i][j] = math.cos(angle)
        elif(i==2 and j==0):
            user_input = b
            angle = float(user_input)
            matrix2[i][j] = -(math.sin(angle))
        elif(i==0 and j==2):
            user_input = b
            angle = float(user_input)
            matrix2[i][j] = (math.sin(angle))
for i in range(3):
    for j in range(3):
        if((i==0 and j==2) or (i==1 and j==2) or (i==2 and j==0) or (i==2 and j==1)):
            matrix3[i][j]=0
        elif(i==2 and j==2):
            matrix3[i][j]=1
        elif((i==0 and j==0) or (i==1 and j==1)):
            user_input = c
            angle = float(user_input)
            matrix3[i][j] = math.cos(angle)
        elif((i==0 and j==1)):
            user_input = c
            angle = float(user_input)
            matrix3[i][j] = -(math.sin(angle))
        elif(i==1 and j==0):
            user_input = c
            angle = float(user_input)
            matrix3[i][j] = (math.sin(angle))
            
            
result = [[0 for i in range(3)] for j in range(3)]

# multiply matrix1 and matrix2
for i in range(3):
    for j in range(3):
        for k in range(3):
            result[i][j] += matrix1[i][k] * matrix2[k][j]

# multiply result and matrix3
final_result = [[0 for i in range(3)] for j in range(3)]
for i in range(3):
    for j in range(3):
        for k in range(3):
            final_result[i][j] += result[i][k] * matrix3[k][j]
            
# print the final result of 3x3
#for row in final_result:
 #  print(row)

#diameter matrix input take
for i in range(3):
    for j in range(1):
        if(i==2 and j==0):
            dia[i][j]=0
        elif(i==0 and j==0):
            user_input = diax
            x = float(user_input)
            dia[i][j]=x
            
        elif(i==1 and j==0):
            user_input = diay
            y = float(user_input)
            dia[i][j]=y
            
#for row in dia:
 #   print (row)   

# multiply final_result with a 3x1 matrix
mulresult = [[0 for i in range(1)] for j in range(3)]
for i in range(3):
    for j in range(1):
        for k in range(3):
            mulresult[i][j] += final_result[i][k] * dia[k][j]
            

#for row in mulresult:
 #   print(row)
#lenght of base link

base[0][0]=b1
base[1][0]=b2
base[2][0]=b3

#for i in base:
   # print(i)
    
#length of link
link[0][0]=l1
link[1][0]=l2
link[2][0]=l3

#for i in link:
 #   print(i)
 
#centre to top length
for i in range(3):
    for j in range(1):
        if(i==2 and j==0):
            user_input = centrez
            z = float(user_input)
            centre[i][j]=z
        elif((i==0 and j==0)or(i==1 and j==0)):
            centre[i][j]=0
#for i in centre:
 #   print(i)

    
# subtracting and adding link,centre,bae from mulresult
mainresult = [[0 for i in range(1)] for j in range(3)]
for i in range(3):
    for j in range(1):
        mainresult[i][j] = mulresult[i][j] +centre[i][j] - base[i][j] -link[i][j]
        
for i in mainresult:
    print("the final output is:",i)

```



