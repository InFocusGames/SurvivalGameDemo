19Survival Game

Code written by Scott Claessens
Email: scott.claessens@hotmail.co.uk



Game schedule:

1) HERD BIRTHS AND DEATHS. Each player's herd grows by a percentage, sampled from the Gaussian
     distribution specified by the parameters from session configs. Then, each player receives a shock,
     with a certain probability. The probability of shocks, the likelihood that they are correlated between
     ALL players, and the size (percentage reduction) of the shocks are all specified in the session configs.

2) REQUEST. If there are other alive players to request from, players can request cattle
     from another player. Players must specify exactly how many cattle they want to request.
     If there is more than one other alive player, they must also specify exactly who they
     want to request from.

3) FULFILL REQUESTS. Players may fulfill the requests of others if they want. They are
     told how much a specific player is requesting, and can then give that player
     more / less than the requested amount, or nothing.

4) END OF YEAR. If there were cattle transfers, all players are informed of them. Then,
     the years ends. Final herd sizes are calculated, and if this falls beneath the minimum
     herd size threshold (specified in session configs), players are warned that they must
     increase it over the threshold or they run the risk of dying and being removed from the game.
     The amount of years, in a row, players can go with their herd under the minimum threshold
     is specified in the session configs.



How to run the game:

1) Install oTree.

Visit http://otree.readthedocs.io/en/latest/install.html to learn how to install the latest version of oTree.
You may also need to install a recent version of Python.

2) Run oTree in your computer's command line.

- Use 'cd' in the command line to set the working directory as the folder this README is saved in.
- Run 'otree resetdb' to reset the database.
- Run 'otree runserver' to run a local server, in which you can demo the game locally.
- Access http://127.0.0.1:8000/demo/ to demo the game locally when the server is running.

3) To run an actual experiment, you will need to push the game online, rather than using the local server.

- The oTree docs give all the information on how to use Heroku (or other platforms) to push games
  online and run experiments.
- Be sure to cite oTree if you use this game to collect data!
  Citation: Chen, D.L., Schonger, M., Wickens, C., 2016. oTree - An open-source platform for laboratory,
  online and field experiments. Journal of Behavioral and Experimental Finance, vol 9: 88-97



Game parameters (session configs):

The following game parameters can be edited before beginning an oTree session (default values used for demos).

NOTE: Default parameters are based on the original model of need-based transfers,
    Aktipis, C. A., Cronk, L., & de Aguiar, R. (2011). Risk-pooling and herd survival: an agent-based model of
    a Maasai gift-giving system. Human Ecology, 39(2), 131-140. Locally-run demo versions of the game will
    automatically use these default parameters.

NOTE2: 'Number of players per group' & 'number of rounds' are hard-coded "constants" in oTree, and so they cannot
    be edited as simply as the parameters below. By the default they are set to 3 and 20, respectively.
    To edit them, you will need to edit the code for the game directly.
    1) Go to SurvivalGame -> models.py
        a. Edit "players_per_group" in Constants class
        b. Edit "num_rounds" in Constants class
    2) Go to settings.py
        a. Edit "num_demo_participants" if you intend on running local demos with a new number of participants
    3) You will need to then reset the database before playing the game: "otree resetdb" in command line

| PARAMETER                        | DEFAULT (RANGE) | DESCRIPTION                                                     |
|                                  |                 |                                                                 |
| initialherd                      | 70              | The number of cattle that players start the first year with     |
|                                  |                 |                                                                 |
| minherd                          | 64              | The minimum threshold for herd size. If a player's herd size    |
|                                  |                 | goes BELOW this amount (equal is okay), they receive a warning  |
|                                  |                 | and are told to increase it over the threshold. If they remain  |
|                                  |                 | under the threshold for a certain number of years, they "die"   |
|                                  |                 | and are removed from the game (see years_before_death)          |
|                                  |                 |                                                                 |
| growth_rate_mean                 | 0.034           | The mean of the Gaussian distribution from which growth rate is |
|                                  |                 | drawn. This value is distinct for each player, in each year     |
|                                  |                 |                                                                 |
| growth_rate_sd                   | 0.0253          | The standard deviation of the Gaussian distribution from which  |
|                                  |                 | growth rate is drawn. This value is distinct for each player,   |
|                                  |                 | in each year                                                    |
|                                  |                 |                                                                 |
| shock_rate                       | 0.1 (0 - 1)     | The probability that a shock will occur on a given year.        |
|                                  |                 | 0 = shocks do not occur, 1 = shocks occur every year. This      |
|                                  |                 | value is distinct for each player, if shocks are not correlated |
|                                  |                 | (see shock_correlated)                                          |
|                                  |                 |                                                                 |
| shock_size_mean                  | 0.3             | The mean of the Gaussian distribution from which shock size is  |
|                                  |                 | drawn. This value is distinct for each player, if shocks are    |
|                                  |                 | not correlated (see shock_correlated)                           |
|                                  |                 |                                                                 |
| shock_size_sd                    | 0.1             | The standard deviation of the Gaussian distribution from which  |
|                                  |                 | shock size is drawn. This value is distinct for each player, if |
|                                  |                 | shocks are not correlated (see shock_correlated)                |
|                                  |                 |                                                                 |
| shock_correlated                 | 0 (0 - 1)       | The probability that, if a shock occurs, it will affect all     |
|                                  |                 | individuals identically (i.e. the same value drawn from the     |
|                                  |                 | Gaussian distribution). 0 = shocks are not correlated, 1 =      |
|                                  |                 | shocks are correlated between all individuals (i.e. when a      |
|                                  |                 | shock occurs, it affects all players identically)               |
|                                  |                 |                                                                 |
| shock_predictable                | False (boolean) | If this is True, shocks cease to occur probabilistically, and   |
|                                  |                 | instead occur predictably, every x years (see                   |
|                                  |                 | shock_predictable_count). Setting this to True bypasses both    |
|                                  |                 | shock_rate and shock_correlated, and instead determines the     |
|                                  |                 | occurrence of a shock by counting the number of years since the |
|                                  |                 | player's last one.                                              |
|                                  |                 |                                                                 |
| shock_predictable_years          | 4               | This value is only important if shock_predictable is True. It   |
|                                  |                 | states how often shocks occur (in years) e.g. with              |
|                                  |                 | shock_predictable set to True, and shock_predictable_years set  |
|                                  |                 | to 4, each player will experience a shock every 4 years,        |
|                                  |                 | reliably. Shock size is still calculated independently for each |
|                                  |                 | player, and shocks need not affect players simultaneously       |
|                                  |                 | (the beginning of the 4 year cycle is random for each player).  |
|                                  |                 |                                                                 |
| years_before_death               | 3               | How many years players can go (in a row) under the threshold    |
|                                  |                 | before "dying" and being removed from the game. The first year  |
|                                  |                 | a player ends a year under the threshold counts as the first    |
|                                  |                 | year. For example, for the default value of years_before_death: |
|                                  |                 | if a player is x cattle below the threshold at the end of Year  |
|                                  |                 | 1, they have the rest of Years 2 and 3 to raise their herd size |
|                                  |                 | above the threshold. If not, they will die after Year 3         |
|                                  |                 |                                                                 |
| nonrandom_network_limited_degree | False (boolean) | Determines whether a constrained, limited network structure is  |
|                                  |                 | imposed on groups. If this is set to False, players can request |
|                                  |                 | cattle from ANY other player in the group (unless that player   |
|                                  |                 | has died and has been removed from the game). However, if this  |
|                                  |                 | is set to True, players can only request from and interact with |
|                                  |                 | a limited number of neighbours. The number of neighbours ("the  |
|                                  |                 | degree for each node in the network") each individual is        |
|                                  |                 | connected to is determined by nonrandom_network_k_degree. This  |
|                                  |                 | feature may be useful for very large groups                     |
|                                  |                 |                                                                 |
| nonrandom_network_k_degree       | 4               | This value is only important if nonrandom_network_limited_degree|
|                                  |                 | is True. This value determines how many neighbours are included |
|                                  |                 | in each player's local network. For the default value, in a     |
|                                  |                 | group of six players, Player 3's neighbours would be Players 1, |
|                                  |                 | 2, 4 and 5 (i.e. the four closest players). Player 6 would      |
|                                  |                 | be excluded. This number must be between the group size and 2.  |
|                                  |                 | NOTE: Even numbers should be used here, because odd numbers     |
|                                  |                 | necessarily lead to some one-way connections (e.g. Player 1     |
|                                  |                 | could request cattle from Player 2, but not vice versa)         |
|                                  |                 |                                                                 |
| observability                    | True (boolean)  | Determines whether players see the herd sizes of other players  |
|                                  |                 | (and whether they are below the minimum threshold) on each      |
|                                  |                 | screen as they play. To deactivate, set to False                |
|                                  |                 |                                                                 |
| charts                           | True (boolean)  | Determines whether players see a chart, visually displaying     |
|                                  |                 | their current herd size on each screen. NOTE: HighCharts can    |
|                                  |                 | slow pages down, so this may be worth deactivating. To          |
|                                  |                 | deactivate, set to False                                        |
|                                  |                 |                                                                 |
| summary_box                      | True (boolean)  | Determines whether players see a box summarising the requests   |
|                                  |                 | during the year. To deactivate, set to False                    |
|                                  |                 |                                                                 |
| real_world_currency_per_point    | 0.01            | USD ($) per cattle, used to calculate bonus payment             |
|                                  |                 |                                                                 |
| participation_fee                | 0.50            | Base participation payment, in USD ($)                          |
|                                  |                 |                                                                 |



Dependent variables:

NOTE: These are calculated for each player after a) that player dies, or b) that player completes the final year.
The values are saved in the final round.

| DEPENDENT VARIABLE                          | DESCRIPTION                                                         |
|                                             |                                                                     |
| rounds_survived                             | The numbers of rounds the player survived                           |
|                                             |                                                                     |
| overall_total_amount_requested              | The total number of cattle the player requested                     |
|                                             |                                                                     |
| overall_total_amount_received               | The total number of cattle the player received from others          |
|                                             |                                                                     |
| overall_total_amount_given                  | The total number of cattle the player gave to others                |
|                                             |                                                                     |
| overall_received_given_totaldiff            | The difference between the total number of cattle the player        |
|                                             | gave to others, and the total number of cattle the player           |
|                                             | received from others. Negative numbers means the player gave        |
|                                             | more cattle than they received, across all rounds                   |
|                                             |                                                                     |
| overall_mean_amount_requested               | The mean number of cattle the player requested. Calculated by       |
|                                             | taking the total requested and dividing by rounds_survived          |
|                                             |                                                                     |
| overall_mean_amount_received                | The mean number of cattle the player received. Calculated by        |
|                                             | taking the total received and dividing by rounds_survived           |
|                                             |                                                                     |
| overall_mean_amount_given                   | The mean number of cattle the player gave to others. Calculated by  |
|                                             | taking the total received and dividing by rounds_survived           |
|                                             |                                                                     |
| overall_num_shocks                          | The total number of shocks experienced                              |
|                                             |                                                                     |
| overall_num_requests_made                   | The total number of requests made                                   |
|                                             |                                                                     |
| overall_num_requests_when_under_threshold   | The total number of requests that were made, specifically when the  |
|                                             | player's herd size was under the minimum threshold                  |
|                                             |                                                                     |
| overall_num_requests_responded_to           | The total number of others' requests the player responded to        |
|                                             |                                                                     |
| overall_repetitive_giving                   | When one player gives another some cattle, the receiver becomes in  |
|                                             | "debt" to the sender. Giving cattle back to the sender removes the  |
|                                             | debt, but pushes debt onto the original sender. If a player gives   |
|                                             | cattle to a player who is already in debt to them, this count of    |
|                                             | "repetitive giving" goes up by one. This variable can be thought of |
|                                             | as: the number of rounds in which the player sent cattle to someone |
|                                             | who had not yet repaid a previous gift                              |
|                                             |                                                                     |
| overall_repetitive_asking                   | If a player requests cattle from someone when they are already in   |
|                                             | debt to that person (they need not send anything), this count of    |
|                                             | repetitive asking goes up by one. This variable can be thought of   |
|                                             | as: the number of rounds in which the player requested cattle from  |
|                                             | someone when they had not yet repaid a previous gift from them      |
|                                             |                                                                     |
