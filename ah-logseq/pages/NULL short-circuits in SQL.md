- |**name**|**age**|**num_dogs**|
  |--|--|--|
  |Ace|20|4|
  |Ada|NULL|3|
  |Ben|NULL|NULL|
  |Cho|27|NULL|
- `age <= 20 OR num_dogs = 3`
	- For Ace, `age <= 20` evaluates to `TRUE`, so the result is `TRUE`
	- For Ada, `age <= 20` evaluates to `NULL` but `num_dogs = 3` evaluates to `TRUE`, so the result is `TRUE`.
	- For Ben, `age <= 20` evaluates to `NULL` and `num_dogs = 3` evaluates to `NULL`, so the overall expression is `NULL` which is `FALSE`.
	- For Cho, `age <= 20` evaluates to `FALSE` and `num_dogs = 3` evaluates to `NULL` so the overall expression evaluates to `NULL`.