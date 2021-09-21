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
Since Bulbasaur has 2 types, we calculate the effectiveness of Fire against Bulbasaur as the product of project.get_type_effectiveness("Fire", "Grass") and project.get_type_effectiveness("Fire", "Poison").

**Some Pokémon have only one type. In this case, we simply return the effectiveness of the attack_type against the defender's Type I**

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
