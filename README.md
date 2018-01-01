import hedgeloop.core.indicator
import pandas as pd 
import numpy as np 
import datetime as dt
import matplotlib.pyplot as plt 


MOVING_INTERVAL = [3,10,20,50]#,100,150,200,250]
STEP_DAY = 2
DURATION = 20

####################################Get Data#########################################
def generate_Random_Data(STEP_DAY,DURATION):
	day = []
	closing = []

	for i in range(DURATION):
		date = dt.datetime.today().date() + dt.timedelta(days = i)
		#if not weekend
		if((i+STEP_DAY)%7 != 0 or (i-1+STEP_DAY)%7 != 0):
			day.append(date)
			closing.append(np.random.randint(-10,100))

	# put into a panda series
	return pd.Series(closing,day)
#####################################################################################

#####################################################################################
def compute_moving_data(data,MOVING_INTERVAL):
	mvg_data = np.array([])
	for i in MOVING_INTERVAL:
		#MVG_AVG = hedgeloop.core.indicator.mvg.mvgavg(data,i)
		mvg_data = np.append(mvg_data,hedgeloop.core.indicator.mvg.mvgavg(data,i))
		#print(MVG_AVG)
	print(mvg_data.shape)
	return mvg_data.reshape(len(MOVING_INTERVAL),len(data))
#####################################################################################

#####################################################################################
def all_moving_data(mvg_data):
	all_mvg_data = np.array([])
	rows,columns = mvg_data.shape
	for i in range(rows-1,-1,-1):
		for j in range(i-1,-1,-1):
			all_mvg_data = np.append(all_mvg_data,np.subtract(mvg_data[i],mvg_data[j]))
	n = int((rows*(rows-1))/2)
	all_mvg_data = all_mvg_data.reshape(int(n),columns)
	return(all_mvg_data)

#####################################################################################

#####################################################################################
def compare(data,all_mvg_data):
	mvg_data_diff = np.array([])
	print(mvg_data_diff)
	rows,columns = all_mvg_data.shape
	for i in range(rows):
		mvg_data_diff = np.append(all_mvg_data[i],np.subtract(all_mvg_data[i],data))
	#print(mvg_data_diff)
	#mvg_data_diff = mvg_data_diff.reshape(int(n),columns)
	return(mvg_data_diff)

#####################################################################################

data = generate_Random_Data(STEP_DAY,DURATION)
#print(data.index)
#print(data.values)
print('Dummy Data')
print(data)
print(data.shape)

mvg_data = compute_moving_data(data,MOVING_INTERVAL)
print('moving data')
print(mvg_data)
print(mvg_data.shape)

all_mvg_data = all_moving_data(mvg_data)
print('all possible moving data')
print(all_mvg_data)
print(all_mvg_data.shape)

mvg_data_diff = compare(data,all_mvg_data)
print('comaprision data')
print(mvg_data_diff)
print(mvg_data_diff.shape)


	
print(asdf)
res = []
for i in range(len(data)):
	if(mvg_data[i]-data[i]>0):
		res.append(-1)
	elif(mvg_data[i]-data[i] < 0):
		res.append(1)
	else:
		res.append(0)

print(res)

#plt.bar(day,res)
#plt.show()
#plt.plot(mvg_data,price,'bo')
#plt.show()
