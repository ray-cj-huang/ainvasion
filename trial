from colorfight import Colorfight
import time
import random
import node
from colorfight.constants import BLD_GOLD_MINE, BLD_ENERGY_WELL, BLD_FORTRESS, BUILDING_COST

def calcValue(cell, game): #RISHI
    turns = game.turn
    value = (turns * cell.natural_gold + (game.max_turn - turns) * cell.natural_energy) / (cell.attack_cost)
    return value

def play_game(
        game, \
        room, \
        username, \
        password \
        ):
    # Connect to the server. This will connect to the public room. If you want to
    # join other rooms, you need to change the argument
    game.connect(room = room)

    # game.register should return True if succeed.
    # As no duplicate usernames are allowed, a random integer string is appended
    # to the example username. You don't need to do this, change the username
    # to your ID.
    # You need to set a password. For the example AI, the current time is used
    # as the password. You should change it to something that will not change
    # between runs so you can continue the game if disconnected.
    if game.register(username = username, \
            password = password):
        # This is the game loop
        while True:
            # The command list we will send to the server
            cmd_list = []
            # The list of cells that we want to attack
            my_attack_list = []
            # update_turn() is required to get the latest information from the
            # server. This will halt the program until it receives the updated
            # information.
            # After update_turn(), game object will be updated.
            # update_turn() returns a Boolean value indicating if it's still
            # the same game. If it's not, break out
            if not game.update_turn():
                break

            # Check if you exist in the game. If not, wait for the next round.
            # You may not appear immediately after you join. But you should be
            # in the game after one round.
            if game.me == None:
                continue

            me = game.me

            adjacentCellValues = node.LinkedList() #RISHI
            cellValuesChecked = set()
            building = ''
            # game.me.cells is a dict, where the keys are Position and the values
            # are MapCell. Get all my cells.
            for cell in game.me.cells.values():
                # Check the surrounding position
                for pos in cell.position.get_surrounding_cardinals():
                    if not (pos in cellValuesChecked): #RISHI
                        cell = game.game_map[pos] #RISHI
                        value = calcValue(cell, game) #RISHI
                        cost = cell.attack_cost
                        adjacentCellValues.insert(cost,value,pos)
                        cellValuesChecked.add(pos)

                    # If we can upgrade the building, upgrade it.
                    # Notice can_update only checks for upper bound. You need to check
                    # tech_level by yourself.
                    if cell.building.can_upgrade:
                        print('hi)')
                        if cell.building.is_home or cell.building.level < me.tech_level:
                             print('bye')
                             if cell.building.upgrade_gold < me.gold:
                                 print('rishi')
                                 print(cell.building.upgrade_energy, me.energy)

                                 if cell.building.upgrade_energy < me.energy:
                                        #build fortress if enermy is detected nearby

                                    print ('iscool')
                                    if cell.building.name == 'home':
                                        cmd_list.append(game.upgrade(cell.position))
                                        print("We upgraded ({}, {})".format(cell.position.x, cell.position.y))
                                        me.gold   -= cell.building.upgrade_gold
                                        me.energy -= cell.building.upgrade_energy
                                        print('town hall')

                                    if cell.building.name == 'gold_mine':
                                        if cell.natural_gold * (game.max_turn - game.turn) > cell.building.upgrade_gold:
                                            cmd_list.append(game.upgrade(cell.position))
                                            print("We upgraded ({}, {})".format(cell.position.x, cell.position.y))
                                            me.gold   -= cell.building.upgrade_gold
                                            me.energy -= cell.building.upgrade_energy
                                            print('gold mine')

                                    if cell.building.name == 'energy_well':
                                        if cell.natural_energy * (game.max_turn - game.turn) > cell.building.upgrade_gold:
                                            cmd_list.append(game.upgrade(cell.position))
                                            print("We upgraded ({}, {})".format(cell.position.x, cell.position.y))
                                            me.gold   -= cell.building.upgrade_gold
                                            me.energy -= cell.building.upgrade_energy
                                            print('elixir collector')






                #print(list_of_energy)



            # Send the command list to the server




                # Build a random building if we have enough gold
                if cell.owner == me.uid and cell.building.is_empty and me.gold >= BUILDING_COST[0]:

                    #print('700')
                    if cell.natural_gold * (game.max_turn - game.turn) > 100:
                        if cell.natural_gold > 3:
                            building = BLD_GOLD_MINE
                            cmd_list.append(game.build(cell.position, building))
                            print("We build {} on ({}, {})".format(building, cell.position.x, cell.position.y))
                            me.gold -= 100

                    if cell.natural_energy * (game.max_turn - game.turn) > 100:
                        if cell.natural_energy > 3:
                            building = BLD_ENERGY_WELL
                            cmd_list.append(game.build(cell.position, building))
                            print("We build {} on ({}, {})".format(building, cell.position.x, cell.position.y))
                            me.gold -= 100

                #SPEND ENERGY
                energyToSpend = me.energy // 4

                currentNode = adjacentCellValues.get_head()

                while(currentNode != None):
                    pos = currentNode.get_pos()
                    c = game.game_map[pos]
                    if (currentNode.get_cost() < energyToSpend and c.position not in my_attack_list and c.owner != game.uid):
                        cmd_list.append(game.attack(pos, c.attack_cost))
                        print("We are attacking ({}, {}) with {} energy".format(pos.x, pos.y, c.attack_cost))
                        game.me.energy -= c.attack_cost
                        energyToSpend -= c.attack_cost
                        my_attack_list.append(c.position)
                    currentNode = currentNode.get_next()

                # Send the command list to the server
                result = game.send_cmd(cmd_list)
                print(result)

    # Do this to release all the allocated resources.
    game.disconnect()

if __name__ == '__main__':
    # Create a Colorfight Instance. This will be the object that you interact
    # with.
    game = Colorfight()

    # ================== Find a random non-full rank room ==================
    #room_list = game.get_gameroom_list()
    #rank_room = [room for room in room_list if room["rank"] and room["player_number"] < room["max_player"]]
    #room = random.choice(rank_room)["name"]
    # ======================================================================
    # Delete this line if you have a room from above

    # ==========================  Play game once ===========================
    play_game(
        game     = game, \
        room     = "official_group_A", \
        username = "trialleave", \
        password = "rishi"
    )
    # ======================================================================

    # ========================= Run my bot forever =========================
    #while True:
    #    try:
    #        play_game(
    #            game     = game, \
    #            room     = room, \
    #            username = 'ExampleAI' + str(random.randint(1, 100)), \
    #            password = str(int(time.time()))
    #        )
    #    except Exception as e:
    #        print(e)
    #        time.sleep(2)
