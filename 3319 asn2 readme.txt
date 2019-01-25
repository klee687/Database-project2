
Purpose:
To give you practice:

    drawing ER diagrams
    converting an ER diagram to a relational database
    creating a database using SQL
    creating tables using SQL
    inserting, modifying and deleting data from tables using SQL
    doing a bulk load of a table using a comma separated file

The Problem:

The database you design should be for the following scenario:  The NHL needs to keep track of it's games, officials (refs) and teams.

For teams, you must keep track of the city the team is from (e.g. Toronto), this field will be no longer than 15 letters, the team name (e.g. Maple Leafs), this field will be no longer than 20 letters, and a 2 digit team id (e.g. 12)

For the officials, you must keep track of their first and last names (each will be 20 letters) and their official id (a 2 digit field) and their home city (15 letter field).

For games, you must keep track of the game id (2 digits), the date of the game and the city the game took place in (15 letters field).

Teams play games. A team plays up to 40 games a season. Each game is played by 2 teams. You must keep track of the score for each team in a game.  Teams may never play a game, but if a game is scheduled then it must have 2 teams play in it. Games are officiated by officials (refs). Every game has at least one official,  but may have more. Every game has one official who acts as the head official (so every game will at least 2 officials, a regular one and a head one, HINT: keep track of both  (officiates game and head officiates game) as 2 separate relationships). Some officials will be in our database who have not yet officiated any games. 

 
Instructions:

There are 2 parts for this assignment. In part 1 you will draw an ER diagram and in part 2 you will create the database on your virtual machine using PostgreSQL
Part 1:

For the above scenario, draw an ER diagram using MS Visio or draw.io.   Then save your ER diagram as yourwesternuseridERDiagram.jpg (or .pdf, or .gif). You should show the total or partial participation using the single/double lines and the (min,max) info. Make sure you indicate the primary key and the cardinality. 
Part 2:

Create the following 4 script files (more details to follow):

    File 1: Delete the database and create a new database  and create the tables
    File 2: Insert some data and update the data
    File 3: Query the data
    File 4: Delete some data, add a constraint, add a view

Make sure the SQL commands are UPPER case and the table names and column names are lowercase;
IMPORTANT:

ssh (or putty.ext) over to cs3319.gaul.csd.uwo.ca and do the following:

    from your home directory, create a directory called assignment2 as follows:
    cd ~
    mkdir assignment2
    set the permissions on the assignment2 directory as follows (so that you are the ONLY person who can see this directory):
    cd ~
    chmod go-rwx assignment2
    when you are done testing and ready to run your scripts, put all your script files (you could use filezilla or winscp to move them up to cs3319.gaul.csd.uwo.ca INTO the assignment2 directory or create them WITHIN that directory) into the directory assignment2 and run the 4 yellow highlighted commands below INSIDE the assignment2 directory, this way no one else can see your files! While you are inside your assignment2 directory, do this to make sure permissions are set:
    ls -lg
    chmod go-rwx *.*
    ls -lg

 
File 1 - Creation

    List all the current databases owned by you
    Delete the database called yourwesternuseridassign2db if it exists
    Create a database called yourwesternuseridassign2db
    Connect to (use) that database
    Add this line to make sure no one can see your database except for you and your prof and the t.a.s:
    REVOKE CONNECT ON DATABASE yourwesternuseridassign2db FROM public;
    GRANT CONNECT ON DATABASE yourwesternuseridassign2db to lreid, mshirpou, mfinnie3, mshermin, yliu888;
    List all the tables (should be none initially)
    Create the tables you need with the appropriate types and keys, foreign keys.  Make sure that:
        you create the foreign keys
        if someone tries to delete a team AND that team has played some games, make it so that all the games that team has played also get deleted. 
    List the tables again
    Call this script: yourwesternuseridscript1.txt
    After you have the script working, save the output from it using the following command:
              psql -a -h dbserver -U username -W username < usernamescript1.txt > usernameoutscript1.txt 2>&1
        NOTE: when you perform the above command YOU WILL BE PROMPTED for your password, perhaps several times. Just enter your password as needed and the usernameoutscript1.txt should be create at the end. 
        -a means echo the commands you wrote in the appropriate places with output that is created
        -h means which host to use that holds postgres
        -U means this is  your username
        -W means force the person to enter a password if required
        username (after the -W) is just the name of your default database
        < means pipe everything in the usernamescript1.txt INTO postgres to execute
        > means pipe all the output from the commands in the input script to usernameoutscript.txt
        2>&1 means also pipe any errors (errors go to 2) into the output script file as well rather than to the terminal

File 2 - Insertion and Modification

    Using the Postgres \copy command (It looks like this:  \copy tablename from ...  NOTE: you will need to google to see the syntax for this command), load the following data into the team table:   [http://www.csd.uwo.ca/~lreid/cs3319/assignments/assignment2/fall2017data.txt]
        NOTE: for this command, you will have to copy the above datafile somewhere onto your cs3319.gaul.csd.uwo.ca machine, then use the unix pwd command to see the path name and use the file name in single quotes
    Insert the following data also into the tables
        Teams:
            99, Colorado, Avalanche
            88, New York, Rangers
            78, Ottawa, Senators
            66, Edmonton, Oilers
            your favourite team or a silly made up team
        Games:
            21, Toronto, Feb 18, 2018
            31, Toronto, Dec 20, 2018
            12, Ottawa, Dec 24, 2018
            15, New York, Dec 20, 2018
            13, New York, Jan 7, 2018
            10, Ottawa, Dec 20, 2018
            14, Toronto, Jan 8, 2018
        Officials:
            99, Hugh Grant, Toronto
            22, Courtney Cox, New York
            33, Rosie Odonnell, Ottawa
            66, David Letterman, Edmonton
            11, Regis Philbin,  Toronto
            12, Rosie Cox,  Ottawa
            some other official of your choice
        Game Info (you can use the foreign keys to enter this info):
            Game 21 --> Maple Leaves (score: 3) against Red Wings (score: 2), head official: 11, officials:11, 12
            Game 31 -->  Maple Leaves (score: 6) against  Oilers (score: 2),   head official: 33 officials: 33, 22, 12
            Game 12 -->  Senators (score: 2) against Rangers (score: 3),   head official:22  officials: 22, 33, 66
            Game 15 -->  Rangers (score: 1) against Red Wings (score: 2),   head official: 11 officials: 22, 11
            Game 13 -->  Oilers (score: 5) against Maple Leaves  (score: 2),   head official: 12 officials: 12, 33
            Game 10 -->  Oilers (score: 4) against Rangers  (score: 2),  head official: 12, officials: 11, 12, 33
            Game 14 -->  Maple Leaves (score: 3) against Red Wings (score: 1),   head official: 33 officials: 33, 11
    Now show all the data in each of the tables to prove that your inserts worked.
    Change the name of the Toronto team to be the Maple Leafs rather than the Maple Leaves
    Change the data so that in every game that the Leafs play, they get a score of 3
    Change it so that every game that David Letterman refereed occurred in January, 2018
    Show the data again in all the tables to prove that your updates worked. 
    Call this script: yourwesternuseridscript2.txt
    After you have the script working, save the output from it using the following command:
        psql -a -h dbserver -U username -W username < usernamescript2.txt > usernameoutscript2.txt 2>&1

File 3 - Queries

Write SQL commands that query the data to:

    Show the team name of all the teams
    Show the city of where all games were played with no repeats
    Show all the data in the officials table, but show them in order of first name
    Show the First and Last Name of all officials from Ottawa or New York
    List the last name of all head officials
    List the first and last name of any official who comes from a city with an "on" in the city name
    List  the gameid, the city the game took place in, the team's city and the teams name and the score for that team.  For these results, make sure the 2 teams that played each other are listed immediately after each other (e.g. do NOT put the team that played in game 15, then the team that played in game 31, then the other team that played in game 15, instead make sure you keep the 2 teams in each game right after each other in the resulting table)
    Find the total number of goals scored for by the Rangers (when creating your query, make sure you reference the team name of 'Rangers' in your query)
    Find the average number of goals that each team scored, print out the team name and the average goals.
    List the game id, the game city, the 2 teams city and team names who played in the game for all games where neither of the teams came from the same city as the game was played in and the scores. BONUS: if you can list the 2 teams who played each other on one line with no repeats.  You can use views do this one.  SO output should look like this for the bonus:

     Game ID | First Team |   From   | First Team Score | Second Team |   From   | Second Team Score
    ---------+------------+----------+------------------+-------------+----------+-------------------
     13      | Oilers     | Edmonton |                5 | Maple Leafs | Toronto  |                 3
     10      | Rangers    | New York |                2 | Oilers      | Edmonton |                 4
    List the head official's first and last name for all head officials of Leafs games
    Find the city name and team name of any teams who haven't played any games
    List the city and team name of any teams who have not played the Leafs.
    Find all officials (first and last name) who have refereed for  more than 1 game (Hint: you will have to use the key words Group By and Having)
    To check for bias by the official, find the first name and last name and official's home city and the game id for any official who referred a game where the official comes from a city that is the same city as one of the  teams playing the game.

    Call this script: yourwesternuseridscript3.txt
    After you have the script working, save the output from it using the following command:
        psql -a -h dbserver -U username -W username < usernamescript3.txt > usernameoutscript3.txt 2>&1

File 4 - Deletions/Views

    Create a view that shows the game id, game city, the head officials first and last name and the head officials home city. Then show all the records in this view.
    Put a constraint that no score can be less than 0. Prove that the constraint works!
    Delete the official that you made up using his/her lastname. Show the table BEFORE the delete and AFTER the delete to prove that it worked. 
    Delete all teams who have not played in any games. Show the table BEFORE the delete and AFTER the delete to prove that it worked. 
    Show the gameid, game city, team city, team name and team score for all teams that played games.  Then delete the Maple Leafs team.  Then show the gameid, game city, team city, team name and team score for all teams that played games  In this situation, the on delete cascade has caused a problem, look at the before and after info of the deletion of the Leafs and identify the problem with our data now.  Multiline comments in Postgres start with /* and end with */.  Add a comment at this point to your script to explain in 1 or 2 sentences what problem has been caused by the deletion of the leafs and the on delete cascade.
    Delete the database called yourwesternuseridassign2db if it exists (this way the t.a.s can run your scripts in order again to recreate the database and test your scripts)
    Call this script: yourwesternuseridscript4.txt
    After you have the script working, save the output from it using the following command:
        psql -a -h dbserver -U username -W username < usernamescript4.txt > usernameoutscript4.txt 2>&1

Notes:

    Here is a sample script file for Postgres -->  [http://www.csd.uwo.ca/~lreid/cs3319/assignments/assignment2/sampleScriptPostgres.txt]
        Save this file to your cs3319.gaul.csd.uwo.ca server.
        To run this sample script, you would type this:  
           psql -a -h dbserver -U username -W username < sampleScriptPostgres.txt > junk.txt 2>&1
        I have purposely put one error in the script above so that you could see the output when an error occurs. 
    Do the steps (inserts, deletes, etc...) in the order given
    For any delete or update commands remember to show the table before and after you modified it
