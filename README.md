# GSQL
##The "Hello World" of Select Statement-
CREATE QUERY GetFriends(vertex<User> inputUser) FOR GRAPH Social{
start = {Input User};
Friends = SELECT t FROM Start :s-(IsFriend:e) - user:t;
PRINT Friends;
}
