import pandas as pd
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns
from xgboost import XGBRegressor

from sklearn.metrics import mean_squared_error

plt.style.use('fivethirtyeight')
clolor_pal=sns.color_palette()

data=pd.read_csv('C:\Users\Sara\Downloads\Compressed\PJME_hourly.csv')

#data.head()
#data.tail()
data.sample(10)

data.info()
data set_index('Datetime')
data=data.set index('Datetime')
pd.to_datetime(data.index)
data.info()
data.plot(figsize=(15,5),style='.',color=clolor_pal[0],title='Energy using mgw') plt.show()

# hour - dayofweek-month - quarter - year - dayofyear
data['hour']=data.index.hour 
data ['dayofweek' ]=data.index.dayofweek
data['month']=data.index.month
data['quarter']=data.index.quarter
data['year' ]=data.index.year
data[ 'dayofyear"]=data.index.hour
data

plt.subplots(figsize=(15,5))
sns.boxplot(data=data,x='hour' ,y='PJME_MW')
plt.title('Energy usage by hour')

plt.subplots(figsize=(15,5))
sns.boxplot(data=data,x='month' ,y='PJME_MW')
plt.title('Energy usage by month')

plt.subplots(figsize=(15,5))
sns.boxplot(data=data,x='quarter' ,y='PJME_MW')
plt.title('Energy usage by quarter')

# model
train_set=data.loc[data.index<'01-01-2015']
test_set=data.loc[data.index>='01-01-2025']

fig,ax=plt.subplots(figsize=(15,5))
train_set.plot(ax=ax)
test_set.plot(ax=ax)
ax.axvline('01-01-2015',color='Black' ,ls='--')
plt.legend(['Train_Set' , 'Test_Set'])
plt.title('Train Test Split')
plt.show()

x_train=train_set.drop('PJME_MW',axis=1)
y_train=train_set['PJME_MW']
x_test=test_set.drop('PJME_MW',axis=1)
y_test=test_set ['PJME_MW']
model=XGBRegressor(n_estimator=1000 , early_stopping_round=50)
model.fit(x_train,y_train)

model.score(x_train,y_train)
pre=model.predict(x_test)
mean_squared_error(pre,y_test)
np.sqrt(mean_squared_error(pre,y_test))


