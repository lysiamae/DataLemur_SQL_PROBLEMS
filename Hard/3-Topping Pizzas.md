# 3-Topping Pizzas 
#### [Hard]
  ----
You’re a consultant for a major pizza chain that will be running a promotion where all 3-topping pizzas will be sold for a fixed price, and are trying to understand the costs involved.

Given a list of pizza toppings, consider all the possible 3-topping pizzas, and print out the total cost of those 3 toppings. Sort the results with the highest total cost on the top followed by pizza toppings in ascending order.

Break ties by listing the ingredients in alphabetical order, starting from the first ingredient, followed by the second and third.

P.S. Be careful with the spacing (or lack of) between each ingredient. Refer to our Example Output.

Notes:
- Do not display pizzas where a topping is repeated. For example, ‘Pepperoni,Pepperoni,Onion Pizza’.
- Ingredients must be listed in alphabetical order. For example, 'Chicken,Onions,Sausage'. 'Onion,Sausage,Chicken' is not acceptable.

####Table: pizza_toppings 
|Column Name| Type|
|----|----|
|topping_name|	varchar(255)
|ingredient_cost|	decimal(10,2)

#### pizza_toppings Example Input:
|topping_name|	ingredient_cost|
|-----|----|
|Pepperoni|	0.50
|Sausage|	0.70
|Chicken|	0.55
|Extra Cheese|	0.40


#### Example Output:
|pizza|	total_cost|
| ----| ---|
|Chicken,Pepperoni,Sausage|	1.75
|Chicken,Extra Cheese,Sausage|	1.65
|Extra Cheese,Pepperoni,Sausage|	1.60
|Chicken,Extra Cheese,Pepperoni|	1.45







##### Explanation
There are four different combinations of the three toppings. Cost of the pizza with toppings Chicken, Pepperoni and Sausage is $0.55 + $0.50 + $0.70 = $1.75.

Additionally, they are arranged alphabetically; in the dictionary, the chicken comes before pepperoni and pepperoni comes before sausage.
The dataset you are querying against may have different input & output - **this is just an example!**




My solution is shown below.
### SOLUTION: 
```sql
SELECT CONCAT(A.topping_name, ',', B.topping_name, ',', C.topping_name) AS pizza,
(A.ingredient_cost + B.ingredient_cost + C.ingredient_cost) as total_cost
FROM pizza_toppings A
CROSS JOIN pizza_toppings B
CROSS JOIN pizza_toppings C 
where A.topping_name < B.topping_name
AND B.topping_name < C.topping_name
ORDER BY total_cost DESC, pizza;

```

The CROSS JOIN was used to make three duplicates of the table pizza_toppings. To ensure that no rows will have the same pizza topping, we introduced the WHERE condition. The where condition ensures that no pizza toppings of the same name will be on the same row, and that the combined list of ingredient per row will be in alphabetical order (e.g. combination Chicken, Pepperoni, Sausage is alphabetically arranged.)



