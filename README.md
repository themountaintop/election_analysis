# Election Analysis

## Purpose

The purpose of this project was to create a way to automatically tally votes, show how many votes each candidate received, show the percentage of the total votes the candidates received, and to show which candidate won the election. This info, along with which county had the largest turnout and the amount they had, would then be displayed in the election_analysis.txt file located in the "analysis" folder.

## Election Info

* Total number of votes cast in the congressional election:
  * 369,711

* County with the largest turnout:
  * Denver County
  * Total Turnout: 306,055

* Breakdown of candidates, percentage of votes received, and total number of votes received:  
  * Charles Casper Stockham: 23.0% (85,213)
  * Diana DeGette: 73.8% (272,892)
  * Raymon Anthony Doane: 3.1% (11,606)

* Election results
  * Winner: Diana DeGette
  * Vote Count: 272,892
  * Percentage of Total: 73.8%
  * Election Audit Summary
  
## Election Audit Summary

This script calculated all the required information to declare the winner of a specific election provided the data file. In order to use this script to scale for any election, some suggestions are as follows:

* Create input box or search box for the user(s) to indicate where they would like to pull the election data from.
#### Example code (sourced from https://stackoverflow.com/questions/20865010/how-do-i-create-an-input-box-with-python):
```
from tkinter import *

master = Tk()
e = Entry(master)
e.pack()

e.focus_set()

def callback():
    print e.get() # This is the text you may want to use later

b = Button(master, text = "OK", width = 10, command = callback)
b.pack()

mainloop()
```
#### Example Output:

![Screen Shot 2022-09-14 at 11 15 04 AM](https://user-images.githubusercontent.com/58227052/190194615-0b711ac3-fa22-4e5a-a121-589ccd721ce8.png)

The above is for example only and the path the user gives would need to be stored for later use.

* 
