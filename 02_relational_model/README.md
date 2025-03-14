# Relational Model
数据模型是描述数据、数据联系、数据语义以及一致性约束的概念工具的集合。本周我们学习的是**关系模型**。此外，为了理解关系数据库查询语言的理论基础，我们还学习了**关系代数**。

# 本周作业（第1次作业）
## 题目一（3分+4分）
考虑一个`银行`数据库，其关系模式如下所示：

- branch (branch_name, branch_city, assets)
- customer (ID, customer_name, customer_street, customer_city)
- loan (loan_number, branch_name, amount)
- borrower (ID, loan_number)
- account (account_number, branch_name, balance)
- depositor (ID, account_number)

使用`关系代数`完成下面的查询：

1. 找到位于`成都`市的支行的名字。
π branch_name (σ branch_city = '成都市' (branch))
SELECT branch_name
FROM 分支
WHERE branch_city = '成都';

2. 找到在`杨柳`支行有贷款（`loan`）的借款人（`borrower`）的ID。
π ID (σ branch_name = '杨柳' (loan ⋈ borrower))   
SELECT borrower.ID
FROM 贷款
JOIN borrower ON 贷款.loan_number = borrower.loan_number
WHERE 贷款.branch_name = '杨柳';   

## 题目二（3分）
假设数据库中存储用户名和密码的关系模式是 users(name, pswd, gender)，请结合关系代数简述实现`用户登录`逻辑的思路。

先进行选择操作（σ），筛选出同时满足 name = 输入用户名 和 pswd = 输入密码 的记录
然后进行投影操作（π），如果只需要返回用户名（或根据需求返回其他字段如 gender），则使用投影操作提取特定属性。若结果非空 → 登录成功；若结果为空 → 登录失败。

σ name=输入用户名 ∧ pswd=输入密码(users)
SELECT name
FROM users
WHERE name = '输入的用户名' AND pswd = '输入的密码';
