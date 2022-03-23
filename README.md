# The "Hello World" of Select Statement-
CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
Start = {Input User};
Friends = SELECT t FROM Start:s-(IsFriend:e) - User:t;
PRINT Friends;
}
 
# WHERE Clause
CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
Start = {Input User};
Friends = SELECT t FROM Start:s-(IsFriend:e) - User:t;
        WHERE e.connectDt BETWEEN to_datetime("2018-01-01") AND to_datetime("2019-01-01")
        AND t.gender == "F";
PRINT Friends;}

# ACCUMULATOR Clause
## SUM, AVG, MIN, MAX accum-
 
 CREATE QUERY SumAvgMinMax() FOR GRAPH social{
 
 SumAccum<Int> @@Sum=2;
 AvgAccum @@Avg=2;
 MinAccum<Int> @@Min=2;
 MaxAccum<Int> @@Max=2;
 
 @@Sum+=1;
 @@Sum+=3;
 @@Sum+=5;
 
 @@Avg+=1;
 @@Avg+=3;
 @@Avg+=5; 

 @@Min+=1;
 @@Min+=3;
 @@Min+=5;
 
 @@Max+=1;
 @@Max+=3;
 @@Max+=5;
 
 print @@Sum;
 print @@Avg;
 print @@Min;
 print @@Max;
 
 O/P= @@Sum:11, @@Avg:2.75, @@Min:1, @@Max:5
 ## SET,LIST,BAG accum-
 
