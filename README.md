# Stocks-Time-Series
Data evaluation and pre-processing of four stocks with their time series data

Following are the 4 time series used from the Yahoo! Finance website:
1. Baron Oil Plc (BOIL.L)
2. Premier African Minerals Limited (PREM.L)
3. Iconic Labs Plc (ICON.L)
4. Lloyds Banking Group Plc (LLOY.L)

#Description
Each of the four time series have 23 missing data points, besides weekends. Since all
four stocks are primarily based in the UK, all 23 of these dates fall on a holiday such as:
New Year’s, Christmas, Good Friday, Easter Monday, Early May Bank Holiday, Late May
Bank Holiday, Summer Bank Holiday and Boxing Day.

Since all four time series have the same missing values, the dates are discarded.

The time series has then been normalized using the x - mean(x) / std(x)

Log return is calculated as ln(w(t)/w(t-1)), where w(t) a data in time t and w(t-1) is the data in time t-1. Ln
is the log function with base e. Since the first row will have no “t-1”, the row is dropped.

Tolerance is calculated as the product of the log return’s variance and one of the three mean square error tolerance levels (0.01,0.5,0.10) (1%, 5% or 10%)

In order to perform calculations (in python) on the “date” column of the dataframe, the dates
are converted into numerical format for easier manipulation.
First the log return values of length n are split into n/2 segments, such that each segment has
two consecutive datapoints. The last segment will have two or three datapoints, depending
on the length of n.

The log return values with k or n/2 segments are stored in the “Output” list, and its
corresponding numerical date values are stored in the “Time” list.
At first, error of each segment will be zero since there are only two datapoints.
A variable “totalErrOld” holds the total error of the segments and “totalErr” is assigned some
large value.

A while loop has been written which will run for about 400 times, which is more than the
length of the segments created. The loop will always be broken due to the tolerance being
reached.

Another loop has been written under it that will run until the length of Output.
The first two consecutive segments along with their corresponding values in the “Time” list
are sent to the function “linreg” which returns the sum of squared errors of the two segments.
This returned error Err1 is added to the “totalErrOld” and the values of the individual errors
of the two segments are removed. This is then compared with the “totalErr” variable.

At the first iteration, the “totalErr” will always be greater, and therefore, this new value is
assigned to it, and the index of the two segments are appended to some list “index”.
This loop runs until all the consecutive segments have been visited and their errors calculated.
The one that gives the least “totalErr” is merged and the Output and Time lists are updated
accordingly. The error of this new segment would be the Err1 calculated when comparing.
The index list stores the index of this new segment’s old individual errors and by simple linear
manipulation, the new error can be calculated.

The “totalErrOld” is updated to include this new error and delete the two individual ones.
This while loop breaks if the totalErr is equal to or greater than the tolerance.
The outer loop will update the totalErr to some random high number again, and the loop is
iterated. The outer loop also breaks when the totalErr is equal to or greater than the
tolerance.
