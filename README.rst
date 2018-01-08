==================================
WoT Console: Player battle counter
==================================

I always thought it was strange that Wargaming did not have an official player
tracker that many other Triple-A developer studios offered. The community has
always volunteered to step up to the plate and fill in this void. While there
are many sites for tracking WN8, per-tank statistics, and community
performance, there still isn't a simple tool for seeing how active the
playerbase is.

So I did what I do best: I question authority, get upset by
`people half-assing answers to questions <http://forum-console.worldoftanks.com/index.php?topic/187259-exploring-wgs-database/page__st__80__pid__3891057#entry3891057>`_
, then I make the means to
`prove someone wrong <http://forum-console.worldoftanks.com/index.php?/topic/187259-exploring-wgs-database/page__st__120__pid__3905673#entry3905673>`_
.

The Battle Counter
==================

This project is set up to store data in multiple databases. The code is written
in Python and uses MySQL for storing player statistics. It is optional to use
Elasticsearch as a search engine for reviewing statistics with simple queries.
Detailed information on how I set this up can be found `on the project Github
pages. <https://kamakazikamikaze.github.io/wotcbattlecounter>`_

To run, pass in a configuration file to the script:

    python src/collect.py config/example.json

If you need to create a new configuration file, a method in utils.py can create
a template for you:

    python -c 'import sys; sys.path.append("./src"); from utils import create_config; create_config("config/example.json");'

Bugs
====

On January 3rd, 2018, I encountered an issue where the SQL database would throw
an error once the code ran. I am not sure what causes it, but I suspect it may
have something to do with the triggers I've set up.

    Error for players: ['6500', '6501', '6502', '6503', '6504', '6505', '6506', '6507', '6508', '6509', '6510', '6511', '6512', '6513', '6514', '6515', '6516', '6517', '6518', '6519', '6520', '6521', '6522', '6523', '6524', '6525', '6526', '6527', '6528', '6529', '6530', '6531', '6532', '6533', '6534', '6535', '6536', '6537', '6538', '6539', '6540', '6541', '6542', '6543', '6544', '6545', '6546', '6547', '6548', '6549', '6550', '6551', '6552', '6553', '6554', '6555', '6556', '6557', '6558', '6559', '6560', '6561', '6562', '6563', '6564', '6565', '6566', '6567', '6568', '6569', '6570', '6571', '6572', '6573', '6574', '6575', '6576', '6577', '6578', '6579', '6580', '6581', '6582', '6583', '6584', '6585', '6586', '6587', '6588', '6589', '6590', '6591', '6592', '6593', '6594', '6595', '6596', '6597', '6598', '6599']
    
    (_mysql_exceptions.IntegrityError) (1062, "Duplicate entry '6562' for key 'PRIMARY'") [SQL: u'UPDATE players SET last_battle_time=%s, updated_at=%s, battles=%s, _last_api_pull=%s WHERE players.account_id = %s'] [parameters: (datetime.datetime(2018, 1, 5, 3, 4, 6), datetime.datetime(2018, 1, 5, 3, 5, 12), 309, datetime.datetime(2018, 1, 5, 4, 11, 36, 378511), 6562L)] (Background on this error at: http://sqlalche.me/e/gkpj)
    
    mysql> select * from players where account_id=6562;
    
    +------------+--------------+---------+---------------------+---------------------+---------------------+---------+---------------------+
    | account_id | nickname     | console | created_at          | last_battle_time    | updated_at          | battles | _last_api_pull      |
    +------------+--------------+---------+---------------------+---------------------+---------------------+---------+---------------------+
    |       6562 | I Djfusion I | xbox    | 2014-02-05 23:12:49 | 2018-01-05 02:56:54 | 2018-01-05 02:56:54 |     308 | 2018-01-05 03:01:37 |
    +------------+--------------+---------+---------------------+---------------------+---------------------+---------+---------------------+
    1 row in set (0.00 sec)

If anyone can determine the root cause and propose a fix, I will implement it
and may restore my project. Until then, I have ceased collecting this data
myself and will remove my servers from Amazon AWS.
