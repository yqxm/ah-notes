- ```C++
  std::set<int> s{1,2,3,4,5};
  std::set::iterator iter = s.begin();
  ++iter;
  *iter;
  iter != s.end();
  auto second_iter = iter;
  ```