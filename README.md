# ai
machine conciousness

import numpy as np 
import pandas as pd 
import matplotlib.pyplot as plt 


print('------------------------------------------------------------------------------')
data = {'length':[1,2,3,4,5,6,7,8,9],'price':[3,6,9,12,15,18,21,24,27]}
frame = pd.DataFrame(data,index=np.arange(1,10,1),columns=['length','price'])
#we don't need feature scaling.
#frame['length'] = ((frame['length'] - frame['length'].mean() ) / (frame['length'].max() - frame['length'].min()))
#frame['price'] = ((frame['price'] - frame['price'].mean() ) / (frame['price'].max() - frame['price'].min()))
print(frame)

print('------------------------------------------------------------------------------')
#h(x) = theta0 + theta1 * x

hypothesisFunction = lambda x: theta0 + theta1 * x

def computeJTheta(theta0,theta1):
    #new frame columns containing (h(x)-y) values
    frame['x'] = (frame['length'].apply(hypothesisFunction) - frame['price'])
    #compute jThetaValue
    val1 = (((frame['x'] ** 2).sum())/(2*m))
    #compute differential of jTheta
    val2 = ((frame['x']).sum()/m)
    #compute differential of jTheta*x
    val3 =((frame['x']*frame['length']).sum()/m)
    return val1,val2,val3
#initial parameters values
#m = frame['length'].count()
m = len(frame)
theta0 = 0
theta1 = 0
alpha = 0.06
#create list for storing theta0 theta1 and JTheta values
jTheta = []
cTheta0 = []
cTheta1 = []
jThetaValue = 1
Accuracy = 0.00001 
#display alpha and accuracy
print('\'alpha\':{0} and Accuracy:{1} '.format(alpha,"%.16f" % Accuracy))
#applyting algorithm
while jThetaValue >= Accuracy :
    jThetaValue,derivativeJTheta0,derivativeJTheta1 = computeJTheta(theta0,theta1)
    #updating theta0 and theta1 value
    temp0 = theta0 - alpha * derivativeJTheta0
    temp1 = theta1 - alpha * derivativeJTheta1
    theta0 = temp0
    theta1 = temp1
    #storing values into lists
    jTheta.append(jThetaValue)
    cTheta0.append(theta0)
    cTheta1.append(theta1)

#print(pd.Series(jTheta),pd.Series(cTheta0),pd.Series(cTheta1))
print('For better learning rate change \'alpha\' value and reduce no. of iterations')
#plot jTheta Learning rate Vs no. of Iterations
plt.plot(np.arange(0,len(jTheta),1),jTheta)
plt.ylabel('jTheta')
plt.xlabel('No. of Iterations')
plt.title('Learning Rate')
plt.show()

plt.title('Theta Vs JTheta')
#plotting graph theta0 vs jTheta
plt.plot(cTheta0,jTheta,color='c', linestyle='dashed', linewidth=1, marker='o', markerfacecolor='green', markersize='3')
#plotting graph theta1 vs jTheta
plt.plot(cTheta1,jTheta,color='c', linestyle='dashed', linewidth=1, marker='o', markerfacecolor='blue', markersize='3')
#setting labels
plt.xlabel('theta0   &  theta1')
plt.ylabel('jTheta')
plt.show()

print('------------------------------------------------------------------------------')
#for price prediction
minIndex = np.argmin(jTheta)
print('Approximated values theta0 := {0} and theta1 := {1}'.format(cTheta0[minIndex],cTheta1[minIndex]))
#input length
length = int(input('Enter the Length value for its Price Prediction:\t'))
#predict price
price = cTheta0[minIndex] + length * cTheta1[minIndex]
print('Estimated Price:\t'+str(price)+'\n')

print('------------------------------------------------------------------------------')
print('Improve learning rate: change \'aplha\' value (takes less iterations)')
print('Accuracy: If \'aplha\' value is good, increase decimals in \'jTheta\' value')
print('------------------------------------------------------------------------------')
