# Interview Preparation for Quantitative Finance Careers

Resources and strategies for preparing for quant trading, quant research, and quantitative developer interviews.

---

## Programming Practice

### [LeetCode](https://leetcode.com/problemset/)

**Focus Areas for Quant Roles**:
- **Arrays & Hashing**: Fundamental data structure manipulation
- **Two Pointers**: Efficient array traversal
- **Dynamic Programming**: Optimization problems (common in quant interviews)
- **Trees & Graphs**: Traversal algorithms, shortest paths
- **Math & Geometry**: Probability, combinatorics, numerical methods

**Recommended Study Plan**:
1. **Weeks 1-2**: Easy problems (arrays, strings, hash tables)
2. **Weeks 3-4**: Medium problems (DP, trees, graphs)
3. **Weeks 5-6**: Hard problems, time yourself
4. **Ongoing**: Daily practice (1-2 problems)

**Languages**: Python preferred for speed, C++ for high-performance roles

**Top Problems for Quants**:
- Two Sum, Three Sum (arrays, hashing)
- Best Time to Buy and Sell Stock (DP, greedy)
- Coin Change (DP)
- Longest Increasing Subsequence (DP)
- Binary Tree traversals (recursion)
- Merge K Sorted Lists (heaps)
- Implement LRU Cache (design)

---

## Quantitative Finance Problems

### [OpenQuant Questions](https://openquant.co/questions)

**Categories**:
- **Probability & Statistics**: Expected value, variance, distributions
- **Stochastic Processes**: Brownian motion, martingales, Itô's lemma
- **Derivatives Pricing**: Black-Scholes, Greeks, volatility
- **Brainteasers**: Logic puzzles, game theory
- **Market Making**: Bid-ask spreads, inventory management

**Study Approach**:
1. **Understand fundamentals**: Review probability and statistics first
2. **Practice daily**: Solve 2-3 problems per day
3. **Explain solutions**: Practice articulating reasoning clearly
4. **Time yourself**: Simulate interview pressure
5. **Review mistakes**: Understand why you got problems wrong

**Example Problem Types**:
- "What's the expected number of coin flips to get HH?"
- "Price a call option using binomial tree"
- "You observe bid-ask spread widening. What could explain this?"
- "Game: Flip coin, get $2^n for n heads before first tail. How much would you pay?"

---

## Books

### Cracking the Coding Interview
**Author**: Gayle Laakmann McDowell  
**Focus**: Programming interview preparation (Google, Facebook, etc.)  
**Relevant Sections**:
- Data Structures (arrays, linked lists, trees, graphs)
- Algorithms (sorting, searching, DP, recursion)
- Interview strategies and behavioral questions

**For Quant Roles**: Focus on chapters 1-10 (DS & Algos), skip system design unless applying for eng roles.

### Additional Recommended Books

#### **A Practical Guide to Quantitative Finance Interviews**
**Author**: Xinfeng Zhou  
**Coverage**: Probability, stochastic calculus, derivatives pricing, brainteasers  
**Best For**: Quant researcher and trader interviews

#### **Heard on The Street: Quantitative Questions from Wall Street Job Interviews**
**Author**: Timothy Falcon Crack  
**Coverage**: Brainteasers, probability puzzles, finance questions  
**Best For**: Understanding interview question types and practicing mental math

#### **Fifty Challenging Problems in Probability**
**Author**: Frederick Mosteller  
**Coverage**: Classic probability problems  
**Best For**: Building intuition for expected value, conditional probability

---

## Interview Types

### 1. Programming Interviews
**Format**: 45-60 minutes, 1-2 LeetCode-style problems  
**Evaluation**: Code correctness, efficiency (time/space complexity), communication  
**Preparation**: LeetCode medium/hard, practice on whiteboard or Google Docs

**Tips**:
- Think aloud while coding
- Clarify requirements before coding
- Test your code with examples
- Discuss time/space complexity
- Know Python standard library well (`collections`, `itertools`, `heapq`)

### 2. Quant/Probability Interviews
**Format**: 45-60 minutes, 3-5 probability/brainteasers  
**Evaluation**: Problem-solving approach, mathematical rigor, intuition  
**Preparation**: OpenQuant, probability textbooks, practice mental math

**Tips**:
- Draw diagrams or trees for complex problems
- State assumptions clearly
- Explain reasoning step-by-step
- Check answer reasonability (sanity checks)
- Know common distributions (binomial, geometric, normal, exponential)

### 3. Trading/Market Making Interviews
**Format**: Market-making games, "what would you bid/ask?" questions  
**Evaluation**: Risk management, quick thinking, PnL calculation  
**Preparation**: Understand bid-ask spreads, inventory risk, adverse selection

**Example**: 
- "I flip a coin, you can bet \$X on heads. What's your max bet?"
- "Market for AAPL is 150-150.10. How many shares would you buy at 150.05?"

**Tips**:
- Consider edge cases (inventory limits, adverse selection)
- Quantify risk (variance, max loss)
- Think about expected value vs risk-adjusted returns

### 4. Behavioral Interviews
**Format**: 30-45 minutes, questions about past experiences  
**Evaluation**: Culture fit, communication, teamwork, motivation  
**Preparation**: STAR method (Situation, Task, Action, Result)

**Common Questions**:
- "Tell me about a time you worked on a team project"
- "Describe a technical challenge you overcame"
- "Why do you want to work in quantitative finance?"
- "What's a trading strategy you find interesting?"

**Tips**:
- Prepare 3-4 stories showcasing different skills
- Be specific (numbers, outcomes, learnings)
- Show passion for markets and quantitative thinking
- Ask thoughtful questions about the role/firm

---

## Firm-Specific Preparation

### High-Frequency Trading / Market Making
**Firms**: Jane Street, Optiver, SIG, IMC, HRT, Citadel Securities  
**Focus**: Probability, mental math, market-making games, programming speed  
**Resources**: 
- Practice market-making card games
- Speed math drills
- Understanding market microstructure

### Quantitative Research
**Firms**: AQR, Two Sigma, DE Shaw, Citadel  
**Focus**: Statistical modeling, ML, research experience, programming depth  
**Resources**:
- Discuss past research projects in detail
- Know statistical tests (t-test, chi-square, regression)
- ML algorithms (regression, trees, neural nets)

### Quantitative Developer
**Firms**: All of the above + banks (GS, JPM)  
**Focus**: System design, low-latency programming, C++, data structures  
**Resources**:
- LeetCode hard problems
- C++ proficiency (STL, memory management, multithreading)
- Understanding of trading systems architecture

---

## Timeline & Strategy

### 3 Months Before Recruiting
- [ ] Review probability and statistics fundamentals
- [ ] Start LeetCode (easy → medium)
- [ ] Read "Cracking the Coding Interview"
- [ ] Solve 20+ OpenQuant problems

### 1 Month Before Recruiting
- [ ] Daily LeetCode practice (medium/hard)
- [ ] Practice explaining solutions aloud
- [ ] Mock interviews with peers
- [ ] Review course notes (probability, statistics, ML)
- [ ] Prepare behavioral stories (STAR method)

### 1 Week Before Interviews
- [ ] Review common probability distributions
- [ ] Practice mental math
- [ ] Rehearse behavioral answers
- [ ] Research firms (culture, projects, people)
- [ ] Prepare questions to ask interviewers

### Day Before
- [ ] Light review (no cramming)
- [ ] Get good sleep
- [ ] Prepare outfit, materials
- [ ] Review resume thoroughly

---

## Mental Math Practice

Quant interviews often involve quick calculations. Practice:
- **Multiplication**: 23 × 47, 68 × 92
- **Division**: 1234 / 17, 5678 / 23
- **Percentages**: 17% of 230, 63% of 480
- **Powers**: 2^10, 1.05^10 (approximations)
- **Square roots**: √50, √200 (approximate)
- **Fractions**: 7/13 + 5/17

**Resources**:
- Mental math apps (Magoosh, Mental Math Cards)
- Daily arithmetic drills (10 minutes)

---

## Resources

### Related Sections
- [Projects](../projects/README.md) - Discuss in behavioral interviews
- [Textbooks](../textbooks/README.md) - Deepen technical knowledge
- [Online Resources](../resources/README.md) - Learn about firms and industry

### External Resources
- [Jane Street Probability & Markets Guide](https://www.janestreet.com/join-jane-street/apply/probability-markets/)
- [Glassdoor Interview Questions](https://www.glassdoor.com/) - Search by firm
- [Wall Street Oasis](https://www.wallstreetoasis.com/) - Interview experiences

---

**Have interview tips to share?** See [Community Contributions](../community/README.md)
