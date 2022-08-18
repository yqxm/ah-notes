- ```C++
  void shift(vector<std::pair<int, int>>& nums) {
    for (size_t i = 0; i < nums.size(); ++i) {
      auto [num1, num2] = nums[i];  // this create a copy
      num1++; //this is updating the copy
      num2++;
    }
  }
  ```
- #+BEGIN_WARNING
  Use `auto &`
  #+END_WARNING