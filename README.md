# The "Hello World" of Select Statement-
CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
 
Start = {inputUser};
 
Friends = SELECT t FROM Start:s-(IsFriend:e) - User:t;
 
PRINT Friends;
}
 
# WHERE Clause
CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
 
Start = {inputUser};
 
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
 }
 
 O/P= @@Sum:11, @@Avg:2.75, @@Min:1, @@Max:5
 
 
 
## SET,LIST,BAG accum-
 
 CREATE QUERY SetListBag() FOR GRAPH social{

 SetAccum<Int> @@Set=2;
 
 ListAccum<Int> @@List=2;
 
 BagAccum<Int> @@Bag=2;
 
 @@Set+=1;
 
 @@Set+=3;
 
 @@Set+=5;
 
 @@Set+=5;
 
 @@List+=1;
 
 @@List+=3;
 
 @@List+=3;
 
 @@List+=5;
 
 @@Bag+=1;
 
 @@Bag+=3;
 
 @@Bag+=5;
 
 @@Bag+=(2,1,2,4);
 
 PRINT @@Set;
 
 PRINT @@List;
 
 PRINT @@Bag;}

 O/P-Set:5,3,1,2

 List:2,1,3,3,5
 
 Bag:4,5,3,1,1,2,2,2
 
 
## Map,Heap accum- 
 
 CREATE QUERY MapHeap() FOR GRAPH social{
 
 TYPEDEF tuple<STRING user, INT value>users;
 
 HeapAccum<users>(3, value DESC)@@Heap;
 
 MapAccum<Int,SumAccum<Int>>@@Map;
 
 @@Heap +=users("UserD",150);
 
 @@Map +=(1->1);
 
 @@Heap +=users("UserB",50);
 
 @@Heap +=users("UserA",100);
 
 @@Heap +=users("UserC",300);
 
 @@Map +=(1->2);
 
 @@Map +=(1->3);
 
 @@Map +=(5->2);
 
 PRINT @@Heap;
 
 PRINT @@Map;} 
 
 O/P: HEAP:
 
 user:USER C
 
 value:300
 
 user:USER D
 
 value:150
 
 user:USER A
 
 value:100
     MAP:1:6,5:2
 
 
# ACCUM Clause
## sum single hop
 CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
 
 MapAccum<uint,SumAccum<uint>>@@ageMap;
 
 Start = {inputUser};
 
 Friends = SELECT t FROM Start:s-(IsFriend:e) - User:t;
 
 WHERE e.connectDt BETWEEN to_datetime("2018-01-01") AND to_datetime("2019-01-01")
 
 ACCUM @@ageMap +=(t.age/10->1);
 
 PRINT @@ageMAP;}
 
 O/P:2->2,3->1
 
## Avg double hop
 CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
 
 AvgAccum @avgAge;
 
 Start={inputUser};
 
 Hop1=SELECT t FROM Start:s-(IsFriend:e)-:t;
 
 Hop2=SELECT t FROM Hop1:s-(IsFriend:e)-:t
 
 ACCUM t.@avgAge +=s.age;
 
 print Hop2;
# POSTACCUM
 CREATE QUERY GetFriends (vertex inputUser) FOR GRAPH Social {
 
 SumAccum @@Num;
 
 SumAccum @@normCNum;
 
 MaxAccum @@maxCNum ;
 
 Start {inputUser} ;
 
 Friends1Hop = SELECT t FROM Start : s- (IsFriend:e) - :t ;
 
 Friends2Hop = SELECT t
 
 FROM Friends1Hop:s- (IsFriend:e) - :t
 
 ACCUM t.@cNum += 1
 
 POST-ACCUM @@maxCNum += t.@cNum;
 
 Friends2Hop = select s FROM Friends2Hop: s
 
 POST-ACCUM
 
 s.@normCNum = s.@cNum/@@maxCNum;
 
 print Friends2Hop;
 }
# HAVING
 CREATE QUERY GetFriends (vertex inputUser) FOR GRAPH Social {
  
 AvgAccum @avgAge;
 
 Start={inputUser};
 
 Hop1=SELECT t FROM Start:s-(IsFriend:e)-:t;
 
 Hop2=SELECT t FROM Hop1:s-(IsFriend:e)-:t
 
 ACCUM t.@avgAge += s.age
 
 HAVING t.@avgAge>30
 
 print Hop2}
# ORDERBY AND LIMIT
 OUTPUT THE FIRST TWO VALUES ORDERED BY INC AVG AGE OF FRIENDS IN COMMON 
 
 CREATE QUERY GetFriends (vertex inputUser) FOR GRAPH Social {
  
 AvgAccum @avgAge;
 
 Start={inputUser};
 
 Hop1=SELECT t FROM Start:s-(IsFriend:e)-:t;
 
 Hop2=SELECT t FROM Hop1:s-(IsFriend:e)-:t
 
 ACCUM t.@avgAge += s.age
 
 ORDER BY t.@avgAge ASC
 
 LIMIT 2;
 
 print Hop2;} 
