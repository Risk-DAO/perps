# vAMM Simulator: how to use

The vAMM simulator uses a csv scenario as inputs. The csv list all actions (trades/repegs/others) that will happen sequentially in the vAMM.

## Install required python dependencies

`pip install -r requirements.txt`

## Run the scenario

`python amm_simulator.py '/path/to/scenario.csv'`

At the end, the simulator will output another csv, it will be placed in the same directory as the input scenario

Example: with input scenario here `'/home/bob/scenario.csv'`, the output csv will be placed here `'/home/bob/output_scenario.csv'`

## Scenario

A scenario describes all the actions that will happen in the amm

### Example

| step | time | action   | vETH | vNFT  | fees | userid | oracle_price
| - | ---- | ------------- |  ---- | ----- | ---- | ------ | ------ |
| 1 | 1669047283 | set_liquidity | 10   | 2000  | 0.01 | admin  | 64.28278325620728
| 2 | 1669047383 | swap          | 1    |       | 0.01 | user1  | 64.28278325620728
| 3 | 1669047483 | swap          | 5    |       | 0.01 | user2  | 64.28278325620728
| 4 | 1669047583 | swap          | 1    |       | 0.01 | user1  | 66
| 5 | 1669047683 | swap          | 1    |       | 0.01 | user3  | 66
| 6 | 1669047783 | set_liquidity | 100  | 20000 | 0.01 | admin  | 66
| 7 | 1669047983 | swap          |      | 200   | 0.01 | user2  | 66


### Columns definition
| label  | description                                                                                                                                   |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| step      | The step where the action took place, can be anything (just a number, a blocknumber…)      
| time      | The unix timestamp (in seconds) where the action took place                                       |
| action | The performed action: set_liquidity or swap. The first action must always be set_liquidity as it sets the base liquidity of the AMM           |
| vETH   | Number of vETH, for set_liquidity action: it means the amount of vETH in the reserve. For swap it means how much vETH will be sold to the AMM |
| vNFT   | Same as vETH but for the other side of the AMM                                                                                                |
| fees   | The trade fees if any                                                                      |
| userid | The user, admin for set_liquidity, userid for a swap, used to track positions  |
| oracle_price   | the oracle price                                                                                                      |


## Output

The output lists, for every steps in the scenario, what happened in the AMM

### Example output
| step             | time       | step_name                                                                         | reserve_vETH | reserve_vNFT      | price (vETH/vNFT) | oracle price      | applied_fees | step_diff_vETH          | step_diff_vNFT           | step_collected_fees_vETH | total_collected_fees_vETH | total_diff_vETH          | total_diff_vNFT        | user_id                                    | cpt_user_long | total_long | cpt_user_short         | total_short            |
| ---------------- | ---------- | --------------------------------------------------------------------------------- | ------------ | ----------------- | ----------------- | ----------------- | ------------ | ----------------------- | ------------------------ | ------------------------ | ------------------------- | ------------------------ | ---------------------- | ------------------------------------------ | ------------- | ---------- | ---------------------- | ---------------------- |
| 0                | 0          | set liquidity: 32133.0 vETH / 500.0 vNFT                                          | 32133.0      | 500.0             | 64.266            | 64.28278325620728 | 0.0          | 0.0                     | 0.0                      | 0.0                      | 0.0                       | 0.0                      | 0.0                    | admin                                      | 0             | 0.0        | 0                      | 0.0                    |
| 39993545         | 1669047283 | 0xde79aaa2f54160b45ef69e1c4b8fc4c290cd5b88 swaps 1.5560327873535e-05 vNFT to vETH | 32132.999    | 500.0000155603279 | 64.26599600000006 | 64.28278325620728 | 0.003        | \-0.0009999999999999445 | 1.5560327873535e-05      | 2.9999999999998336e-06   | 2.9999999999998336e-06    | \-0.0009999999999999445  | 1.5560327873535e-05    | 0xde79aaa2f54160b45ef69e1c4b8fc4c290cd5b88 | 0             | 0.0        | 1                      | 1.5560327873535e-05    |
| 39993929         | 1669047387 | 0xde79aaa2f54160b45ef69e1c4b8fc4c290cd5b88 swaps 0.001 vETH to vNFT               | 32132.999997 | 500.000000046681  | 64.26599998799999 | 64.28278325620728 | 0.003        | 0.000997                | \-1.5513646891363642e-05 | 3,00E-06                 | 5.999999999999834e-06     | \-2.9999999999444533e-06 | 4.6680982171357896e-08 | 0xde79aaa2f54160b45ef69e1c4b8fc4c290cd5b88 | 0             | 0.0        | 1                      | 4.6680982171357896e-08 |
| 40546253         | 0          | set liquidity: 31008.03775 vETH / 500.0 vNFT                                      | 31008.03775  | 500.0             | 62.0160755        | 64.28278325620728 | 0.0          | 0.0                     | 0.0                      | 0.0                      | 5.999999999999834e-06     | \-2.9999999999444533e-06 | 4.6680982171357896e-08 | admin                                      | 0             | 0.0        | 1                      | 4.6680982171357896e-08 |
| 40555181         | 0          | set liquidity: 31856.61661 vETH / 500.0 vNFT                                      | 31856.61661  | 500.0             | 63.71323322       | 64.28278325620728 | 0.0          | 0.0                     | 0.0                      | 0.0                      | 5.999999999999834e-06     | \-2.9999999999444533e-06 | 4.6680982171357896e-08 | admin                                      | 0             | 0.0        | 1                      | 4.6680982171357896e-08 |
| funding_payments | 1669050882 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669054482 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669058082 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669061682 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669065282 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669068882 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
| funding_payments | 1669072482 | funding payment short --> long total payments: 0.0 vETH                           | 31856.61661  | 500.0             | 63.71323322       | 63.98137810183117 | 0.0          | 0.0                     | 0.0                      | 5.999999999999834e-06    | \-2.9999999999444533e-06  | 4.6680982171357896e-08   | admin                  | 0                                          | 0.0           | 1          | 4.6680982171357896e-08 |
### Columns definition
| label                     | description                                               |
| ------------------------- | --------------------------------------------------------- |
| step                      | the step where the action happened                        |
| time                      | unix timestamp where action happened                        |
| step_name                 | A label for the step, describes the human readable action |
| reserve_vETH              | The amount in the vETH reserve AFTER the action           |
| reserve_vNFT              | The amount in the vNFT reserve AFTER the action           |
| price (vETH/vNFT)         | Price                                                     |
| oracle price              | The oracle price                                          |
| applied fees              | the fees applied to the swap (dynamic fees)          |
| step_diff_vETH            | Difference between action t-1 and t for vETH              |
| step_diff_vNFT            | Difference between action t-1 and t for NFT               |
| step_collected_fees_vETH  | Amount of vETH collected as fee for this step             |
| total_collected_fees_vETH | Total of vETH collected since the beginning               |
| total_diff_vETH           | Total in/out for vETH                                     |
| total_diff_vNFT           | Total in/out for vNFT                                     |
| user_id                 | The trader that made the swap                             |
| cpt_user_long             | Amount of users in long position                          |
| total_long                | Amount of vNFT in long position                           |
| cpt_user_short            | Amount of users in short position                         |
| total_short               | Amount of vNFT in short position                          |