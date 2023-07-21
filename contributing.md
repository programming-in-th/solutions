# Contributing to PROGRAMMING.IN.TH's solutions

## Steps

1. [Fork](https://help.github.com/articles/fork-a-repo/) this repository to your own GitHub account and then [clone](https://help.github.com/articles/cloning-a-repository/) it to your local device.
2. Create a new branch `git checkout -b MY_BRANCH_NAME` (`MY_BRANCH_NAME` should be the same with patch you are working on Ex. add_0001 / fix_0001)
3. Open pull request.

## Guidelines

1. Providing code without explaination is **strictly prohibited**
2. General guideline for organizing solution is the following:

- Clearly states the problem(s) at hands
- Provide general solution for each of the problem
- Explains any specific algorithm(s) associated with this problem. (General concepts, such as sorting, Dijkstra's, dynamic programming, can be omitted, but we encouraged to put backlinks in appendix)
- If the previous 2 steps are not enough for explaining, you can provide example code with comments or walkthrough.

3. All mathematical notation must be in latex math format.
4. All in-line codes must in backticks.
5. Code blocks must be formatted in their respective style. The following is just recommendation, but any style that diverts from the recommendation should be indicated in pull request.

- C++: LLVM or Google style
- Rust: rustfmt style
- Java: TBD
- Python: TBD
- Go: gofmt

## Recommendation

- Markdown files will be rendered by a function which is also provided at [programming.in.th/render](https://programming.in.th/render)
