<H1>About our project</H1>

If you want to see the best experience of our project, you need to open terminal and install these modules:
```bash
pip install pandas
pip install matplotlib
```
They will be imported in the main source. See lines below. That is why we can't calculate something later.
```python
# Imports
import pandas as pd
import matplotlib.pyplot as plot
```
In next lines we described all the events and got each of them points of suspiciousness of activity.
```python
# Array of events
events = {"Transaction Refund": 1,
            "Order": 0,
            "Account Setup Skip": 1,
            "Signup Success": 0,
            "Add Vehicle Success": 0,
            "Add Vehicle Break": 1,
            "Add Payment Method Success": 0,
            "Add Payment Method Failed": 2,
            "Email Confirmation Success": 0,
            "Chat Conversation Open": 0,
            "Sign Out": 1,
            "Account History Transaction Details": 0,
            "Wallet Opened": 0,
            "Subscription Premium": 0,
            "Subscription Premiun Cancel": 2,
            "Calculator View": 0}

# Creating massive for user IDs and their info
users = []
info = []
```
Then we are reading dataset with pandas module and display it.
```python
# Reading data
data = pd.read_csv("/content/int20h-ds-test-dataset.csv", delimiter = ",")
data
```
In the next block of code we break off our pandas object: we cut off only two columns from the visual 
  table and iterate them in cycle. While such activity is in the progress, Python check each person ID.
If the person is available in `users` array, the program add corelation points to him. If not - we append
  a new value to arrays `users` and `info` (about these users). The next module is optional.
  It depends on user`s needs to launch them.
```python
# Detecting of suspicious users
for row in (data.loc[:, ["userid", "event_name"]]).itertuples():
    add_corelation = events.get(row.event_name)
    if add_corelation == None: add_corelation = 0
    try:
        user_availability = users.index(row.userid)
    except ValueError:
        user_availability = -1
    if user_availability == -1:
        users.append(row.userid)
        info.append(add_corelation)
    else:
      index = users.index(row.userid)
      info[index] = info[index] + add_corelation


# Creating Pandas table
df = pd.DataFrame(
    {
        "User_ID": users,
        "Cancellation_Score": info,
    }
)
df
#df.to_csv('final.csv')
```
Now we are creating a plot
```python
# Creating a plot for visualizing info
plot.plot(users, info)
plot.show()
```
And display summary of concluded work.
```python
print(f"Найбільш підозріла активність спостерігається від корситувача із ID {users[info.index(max(info))]}. Він набрав {max(info)} балів підозрілості")
```
