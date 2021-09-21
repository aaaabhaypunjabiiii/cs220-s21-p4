# Project 4: Pokémon Simulation

## Learning Objectives

In this project, you will walk through how to

  * Use conditional statements to write programs with more complicated logic
  * Practice writing more functions
  * Begin using good coding practices outlined in Lab P4
  
Please go through Lab P4 before working on this project. The lab introduces some useful techniques related to this project.

## Overview

For this project, you'll be using the data from `pokemon_stats.csv` to simulate Pokémon battles. This data was gathered by the Python program `gen_csv.ipynb` from the website https://www.pokemondb.net/. This project will focus on conditional statements. To start, download `project.py`, `test.py`, `type_effectiveness_stats.csv` and `pokemon_stats.csv`. You'll do your work in a Jupyter Notebook, producing a `main.ipynb` file. You'll test as usual by running `python test.py` to test a `main.ipynb` file.

We won't explain how to use the project module here (the code in the `project.py` file). The lab this week is designed to teach you how it works. So, before starting P4, take a look at Lab P4.

This project consists of writing code to answer 20 questions. If you're answering a particular question in a cell in your notebook, you need to put a comment in the cell so we know what you're answering. For example, if you're answering question 13, the first line of your cell should contain #q13.

In this project, you will have to write several functions and keep adding more details to them according to the instructions. When you are adding more things in your functions, we want you to follow the Good Coding Style for Functions described in Lab P4. Therefore, you should only keep the latest version of your functions in your notebook file. For example, in P4 you are asked to write 6 functions(`original_damage`, `type_effectiveness_against`, `effective_damage`, `num_hits`, `battle`, and `two_on_two_battle`), so there should only be the latest version of these 6 functions in your submitted notebook file.

## Questions and Functions

For the first few questions, we will calculate the damage caused by a Pokémon on another Pokémon. To accomplish this, we need to create a function `original_damage(attacker, defender)`. 

The damage caused by a Pokémon's Physical move is `10 * Attack stat of Attacker / Defense stat of Defender`, and the damage caused by a Pokémon's Special move is `10 * Sp. Atk. stat of Attacker / Sp. Def. stat of Defender`

**If a Pokémon wants to win, it should always choose the move which will do more damage.**

Copy/paste the following code in a new cell of your notebook and fill in the details.

```
def original_damage(attacker, defender):
    physical_damage = 10 * project.get_attack(attacker) / project.get_defense(defender)
    special_damage = ???
    if ???:
        return physical_damage
    else:
        return ???
```
For the following questions, assume the attacking pokemon chooses the move that does the most damage:

## #Q1: How much damage does Scraggy do to Tranquill?
## #Q2: How much damage does Gulpin do to Nidorina?

To make our battles more interesting, we need to measure the effectiveness of a Pokémon's type against another Pokémon. Here, we create a function `type_effectiveness_against(attack_type, defender)`. This function takes a Pokémon's type(like Fire or Water) and a defending Pokémon(like Pikachu) and measures the type's effectiveness against the defending Pokémon. 

This function retrieves Type I and Type II of the defending Pokémon. The effectiveness of the attack_type is measured against each type of the defender using `get_type_effectiveness` which you used in Lab P4. These values are multiplied if the has a 2nd Type(i.e. not "None").

For example, let the attack_type be Fire and the defender be the Pokémon Bulbasaur. Bulbasaur has 2 types, those being Grass and Poison respectively.
```
>>> project.get_type_effectiveness("Fire", "Grass")
2.0
>>> project.get_type_effectiveness("Fire", "Poison")
1.0
```
Since Bulbasaur has 2 types, we calculate the effectiveness of Fire against Bulbasaur as the product of `project.get_type_effectiveness("Fire", "Grass")` and `project.get_type_effectiveness("Fire", "Poison")`. In other words,

`bonus = project.get_type_effectiveness("Fire", "Grass") * project.get_type_effectiveness("Fire", "Poison")`

**Some Pokémon only have one type (their second type is "None"). In this case, we simply return the effectiveness of the attack_type against the defender's first type (Type I). You'll need to check if the defender has a second type before calculating the type effectiveness.**

We've provided the code for this function below:
```
def type_effectiveness_against(attack_type, defender):
    defender_type1 = ???
    defender_type2 = ???
    bonus = 1
    bonus *= project.get_type_effectiveness(attack_type, defender_type1)
    if ???:
        ???
    return bonus
```

Use this function to answer the next 2 questions.

## #Q3: How effective is Electric type against Magikarp?
## #Q4: How effective is Bug type against Gastly?

Upon glancing at `original_damage(attacker, defender)`, we observe that the type effectiveness of each Pokemon is ignored. To resolve this and add a layer of depth to our battles, we create `effective_damage(attacker, defender)`. 

This function is responsible for taking the attacker's type effectiveness into consideration with the damage it causes to a defending Pokemon.

We need to compare the type effectiveness (of each type) of the attacker with the defending Pokemon. The effective damage produced by the attacker is given by:

 `effective damage(attacker) = original_damage(attacker,defender) * the Type with the higher type_effectiveness_against the defending Pokemon.`
 
 To illustrate this, we take Weedle as the attacker and Charizard as the defender.
 If your `type_effectiveness_against` function works correctly the values obtained by measuring the type effectiveness of Mewtwo against Gengar is 
 
```
>>> type_effectiveness_against(project.get_type1("Weedle"), "Charizard")
0.25
>>> type_effectiveness_against(project.get_type2("Weedle"), "Charizard")
1.0
```
Clearly, Weedle's second type or Poison causes more damage to Charizard than its first type or Bug.
We take the greater value of the 2 and multiply it by the original_damage(attacker,defender)

Similar to the previous function, some attacking Pokemon do not have a second type(i.e. their second type is "None"). Your code needs to check if the second type of the attacker is "None" and proceed accordingly. If the second type is "None" we simply return the effectiveness of the first type * the original_damage(attacker,defender)

The code for this function is provided below.

```
def effective_damage(attacker, defender):
    attacker_type1 = ???
    attacker_type2 = ???
    bonus = type_effectiveness_against(attacker_type1, defender)
    if ???:
        if ???:
            ???
    return bonus * ???
```
## #Q5: How much damage does Mewtwo do to Gengar?

## #Q6: How much damage does Wooper do to Charmeleon?

## #Q7: How much damage does Slowpoke do to Metapod?

Now that we have a way of calculating the damage done by the Pokémon during battle, we have to calculate how many hits each Pokémon can take before fainting.

The number of hits a Pokemon can take is calulated by taking its HP and dividing it by the attacking Pokemon's **effective damage**. 

**Hint: If the defending pokemon has 30 HP and the attacking pokemon does 20 damage each turn, it will take 2 turns before the defender faints instead of 30 / 20 = 1.5. You might want to use the method math.ceil() here. First import the module math and then look up the documentation of math.ceil to see how you could use it.**

Copy/paste the following code in a new cell of your notebook and fill in the details.

```
def num_hits(attacker, defender):
    return math.ceil(project.get_hp(???)/???)
```

## #Q8: How many hits can Cottonee take from Wooper?

## #Q9: How many hits can Charmeleon take from Pikachu?

With the tools we have created so far, we can now finally create a battle simulator. Copy/paste the following code in a new cell of your notebook and fill in the details.

```
def battle(pkmn1, pkmn2):
    #TODO: Return the name of the pkmn that can take more hits from the other
    # pkmn. If both pkmn faint within the same number of moves, return the
    # string 'Draw'
```

## #Q10: What is the output of battle('Krabby', 'Scraggy')?
## #Q11: What is the output of battle('Charizard', 'Turtonator')?

You may have noticed that the function battle does not quite follow all the rules that we laid out at the beginning. Look at the output of `battle('Raichu', 'Crobat')`. You will find that it is a draw, since they can both take 4 hits from the other Pokémon. But since Crobat has a higher Speed, it attacks first, so it will land its fourth hit on Raichu, before Raichu can hit Crobat. So, even though they both go down in the same number of moves, Crobat should win the battle.

Go back and modify battle() so that if both Pokémon faint in the same number of moves, the Pokémon with the higher Speed wins. If they both have the same Speed, then the battle should be a 'Draw'.

## #Q12: What is the output of battle('Starly', 'Pidgey')?
## #Q13: What is the output of battle('Bulbasaur', 'Spearow')?

One last rule we need to implement is the run away feature. It is more reasonable to compare the number of hits a Pokémon can take instead of total stats in deciding whether it should run away. For example, consider a battle between Pikachu and Glaceon. Since Glaceon can take 13 hits from Pikachu, but Pikachu can only take 2 hits from Glaceon, Pikachu should run away from this battle.

Modify battle() so that if abs(num_hits_pkmn1_can_take - num_hits_pkmn2_can_take) > 10, the function returns <pkmn_name> ran away'. Make sure the function says the Pokémon that can take less hits ran away!

## #Q14: What is the output of battle('Glaceon', 'Pikachu')?
## #Q15: What is the output of battle('Meditite', 'Gyarados')?
## #Q16: What is the output of battle('Mudkip', 'Gulpin')?
