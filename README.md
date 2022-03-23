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
