MySQL 和 PostgreSQL 是面试中常考的两种数据库系统，尤其是全栈和后端岗位常见。下面按照常见类别，列出了一些面试中常问的 MySQL 和 PostgreSQL 相关问题，并附上简要说明（后续你可以要求我逐题展开）。

---

### 一、基础概念类

1. **MySQL 与 PostgreSQL 有什么区别？**
2. **PostgreSQL 是什么？有哪些特点？**
3. **MySQL 的存储引擎有哪些？InnoDB 和 MyISAM 有什么区别？**
4. **MySQL 和 PostgreSQL 的事务隔离级别有哪些？默认是什么？**
5. **PostgreSQL 如何支持 JSON、数组、HStore 等复杂数据类型？**
6. **MySQL 中 CHAR 与 VARCHAR 的区别？**

---

### 二、索引相关

1. **MySQL 和 PostgreSQL 的 B+ 树索引和哈希索引有什么区别？**
2. **什么是覆盖索引（Covering Index）？PostgreSQL 是否支持？**
3. **PostgreSQL 的 GIN 与 GiST 索引是什么？适用于哪些场景？**
4. **如何选择合适的索引类型？**
5. **MySQL 和 PostgreSQL 是否支持部分索引（Partial Index）？**

---

### 三、事务与锁机制

1. **PostgreSQL 的 MVCC（多版本并发控制）是如何实现的？**
2. **MySQL 的 InnoDB 引擎是如何实现 MVCC 的？**
3. **MySQL 和 PostgreSQL 的行锁、表锁如何使用？**
4. **什么是幻读？PostgreSQL 如何避免？**
5. **PostgreSQL 如何处理死锁？如何检测和避免？**

---

### 四、SQL 编写与优化

1. **PostgreSQL 和 MySQL 的 SQL 语法有何不同？**
2. **Explain 分析如何使用？如何查看 SQL 性能？**
3. **PostgreSQL 的 CTE（WITH 语句） 和递归查询使用方法？**
4. **MySQL 如何优化慢查询？**
5. **PostgreSQL 的窗口函数有哪些使用场景？**

---

### 五、高级功能和特性

1. **MySQL 和 PostgreSQL 对 JSON 的支持有什么不同？**
2. **PostgreSQL 的存储过程与函数有什么区别？**
3. **PostgreSQL 如何实现全文搜索？与 ElasticSearch 有何不同？**
4. **MySQL 支持的主从复制和读写分离机制？**
5. **PostgreSQL 的外部数据封装器（FDW）是什么？能干什么？**
6. **如何实现 PostgreSQL 的逻辑复制和物理复制？**

---

### 六、实践与运维

1. **MySQL 和 PostgreSQL 的备份恢复方式有哪些？**
2. **如何实现 PostgreSQL 或 MySQL 的高可用？**
3. **MySQL 慢查询日志如何开启与分析？**
4. **如何配置 PostgreSQL 的连接池？使用哪些工具？**
5. **PostgreSQL 中 autovacuum 的作用是什么？**

---

### 七、版本与迁移

1. **从 MySQL 迁移到 PostgreSQL 有哪些挑战？**
2. **两者数据类型有何兼容问题？**
3. **使用哪些工具可以实现 MySQL 和 PostgreSQL 的双向同步或迁移？**

---

如你需要，我可以逐类或逐题详细讲解（包括例子、底层原理和优化建议）。
你希望我先讲哪一类？

---

我们从第一类开始详细讲解：**基础概念类**

---

## **一、基础概念类详细讲解**

### 1. **MySQL 与 PostgreSQL 有什么区别？**

| 特性      | MySQL                    | PostgreSQL                               |
| --------- | ------------------------ | ---------------------------------------- |
| 类型      | 关系型数据库             | 面向对象+关系型数据库                    |
| 标准支持  | SQL 支持良好             | 更严格遵循 SQL 标准                      |
| 扩展性    | 插件不多，自定义能力弱   | 支持多种扩展机制（如自定义类型、函数）   |
| JSON 支持 | 有，但功能较弱           | 原生支持 JSON/JSONB，功能强大            |
| 并发控制  | 基于行锁 + MVCC          | 基于多版本并发控制（MVCC），更先进       |
| 索引类型  | B+树为主                 | 支持多种索引类型（B-Tree、GIN、GiST 等） |
| 事务      | 支持 ACID，InnoDB 实现   | 支持 ACID，原生实现                      |
| 复制      | 主从复制（异步、半同步） | 支持物理复制、逻辑复制，更灵活           |
| 社区生态  | 大量使用者，生态成熟     | 企业级使用增长，社区活跃、学术认可度高   |

👉 一句话总结：**MySQL 更适合中小项目和读多写少的场景，而 PostgreSQL 更适合需要复杂查询、高一致性、高并发或高级数据类型的应用。**

---

### 2. **PostgreSQL 是什么？有哪些特点？**

**PostgreSQL 是一个开源、对象-关系型数据库管理系统（ORDBMS），以扩展性和标准兼容性著称。**

#### 核心特点：

- 原生支持 **事务**、**ACID** 保证
- 支持 **自定义函数、类型、操作符、索引、聚合函数**
- 内置支持 **复杂数据类型**：数组、JSON/JSONB、HSTORE、XML、UUID
- 支持 **MVCC 并发控制机制**，提高并发性能
- 支持 **逻辑和物理复制**，适合分布式与高可用部署
- 支持 **全文检索、地理空间数据（PostGIS）**

---

### 3. **MySQL 的存储引擎有哪些？InnoDB 和 MyISAM 有什么区别？**

#### 常见存储引擎：

- **InnoDB（默认）**
- MyISAM（老版本默认）
- MEMORY（内存表）
- ARCHIVE、CSV、BLACKHOLE 等

#### InnoDB vs MyISAM

| 特性     | InnoDB      | MyISAM               |
| -------- | ----------- | -------------------- |
| 事务支持 | ✅ 是       | ❌ 否                |
| 外键支持 | ✅ 是       | ❌ 否                |
| 锁机制   | 行级锁      | 表级锁               |
| 崩溃恢复 | ✅ 自动恢复 | ❌ 需手动修复        |
| 表空间   | 独立表空间  | 单文件               |
| 适用场景 | 高并发写入  | 读多写少、分析型业务 |

---

### 4. **MySQL 和 PostgreSQL 的事务隔离级别有哪些？默认是什么？**

SQL 标准定义了 4 个隔离级别：

| 隔离级别         | 说明                         | 常见问题                      |
| ---------------- | ---------------------------- | ----------------------------- |
| Read Uncommitted | 最低级别，允许读取未提交数据 | 脏读、不可重复读、幻读        |
| Read Committed   | 只读取已提交数据             | 不可重复读、幻读              |
| Repeatable Read  | 多次读取数据一致             | 幻读（MySQL InnoDB 特殊处理） |
| Serializable     | 最严格，完全隔离             | 性能下降                      |

#### 默认隔离级别：

- **MySQL（InnoDB）**：Repeatable Read（可重复读）
- **PostgreSQL**：Read Committed（已提交读）

✅ PostgreSQL 通过 **MVCC + 行版本控制** 来防止脏读和不可重复读。

---

### 5. **PostgreSQL 如何支持 JSON、数组、HStore 等复杂数据类型？**

- **JSON / JSONB**：原生支持 JSON 存储和查询（JSONB 为二进制格式，查询更快）

  ```sql
  SELECT data->'name' FROM users WHERE data->>'age' > '25';
  ```

- **数组类型**：

  ```sql
  CREATE TABLE tags (id serial, tag_list text[]);
  SELECT * FROM tags WHERE 'pg' = ANY(tag_list);
  ```

- **HStore**：键值对类型，类似 JSON，但更轻量

  ```sql
  CREATE EXTENSION hstore;
  SELECT data -> 'key1' FROM kv_store;
  ```

👉 PostgreSQL 的复杂数据支持是其强大优势之一，非常适合半结构化数据。

---

### 6. **MySQL 中 CHAR 与 VARCHAR 的区别？**

| 特性     | CHAR                           | VARCHAR                        |
| -------- | ------------------------------ | ------------------------------ |
| 类型     | 定长                           | 可变长                         |
| 存储空间 | 固定大小                       | 实际数据 + 1\~2 字节长度标记   |
| 适用场景 | 固定长度字段，如国家码、状态码 | 变化较大的字段，如用户名、地址 |
| 查询性能 | 稍快（定长）                   | 稍慢（变长）                   |
| 补齐行为 | 会自动补空格填充到固定长度     | 不自动补齐空格                 |

---

如果你觉得节奏合适，我可以继续讲下一类：**索引相关类问题**，是否继续？

---

好的，我们继续讲解第二类：**索引相关类问题**。

---

## **二、索引相关类详细讲解**

### 1. **MySQL 和 PostgreSQL 的 B+ 树索引和哈希索引有什么区别？**

#### B+ 树索引（默认类型）

- **结构**：多层平衡树，所有数据都存储在叶子节点，非叶子节点只存索引键。
- **适用范围**：大多数范围查询、排序、等值查询。
- **支持的操作**：范围查找 (`BETWEEN`)、前缀模糊匹配（如 `LIKE 'abc%'`）。

#### 哈希索引（Hash）

- **结构**：通过哈希函数将键映射到固定位置。
- **适用范围**：等值查找（=），**不支持范围查询、排序、模糊匹配**。
- **MySQL**：只有 Memory 引擎支持 Hash 索引。
- **PostgreSQL**：8.x 之前不推荐使用，10+ 开始支持并自动 WAL 日志记录。

✅ 总结：

- **优先使用 B+ 树索引**，除非是极少量数据并且只做等值查询时可以考虑 Hash。
- PostgreSQL 在功能强大的情况下，**还支持更多索引类型**（见下文）。

---

### 2. **什么是覆盖索引（Covering Index）？PostgreSQL 是否支持？**

**覆盖索引** 是指：**查询所需的所有字段都可以从索引中获取，不需要回表（访问原表）**。

#### MySQL 示例：

```sql
-- 假设 idx_name_age 是 (name, age) 的联合索引
SELECT name, age FROM users WHERE name = 'Tom';
```

此时如果只读取 name 和 age，查询可以**完全在索引中完成**，效率更高。

#### PostgreSQL：

- 虽然默认索引结构不会包含非键字段，但 **支持 `INCLUDE` 索引（PostgreSQL 11+）**：

```sql
CREATE INDEX idx_name ON users(name) INCLUDE (age);
```

✅ PostgreSQL 的 INCLUDE 索引就是为实现 Covering Index 而设计的。

---

### 3. **PostgreSQL 的 GIN 与 GiST 索引是什么？适用于哪些场景？**

PostgreSQL 支持的多种索引类型是其一大优势：

#### GiST（Generalized Search Tree）通用搜索树

- 支持模糊匹配、全文搜索、地理位置（PostGIS）
- 适用于范围查询、自定义数据类型、KNN 搜索等
- 使用场景：

  - 多边形交集判断
  - 模糊匹配（如 `LIKE '%abc%'`）——需配合 trigram 扩展

#### GIN（Generalized Inverted Index）通用倒排索引

- 更适合**包含多值的字段**，比如数组、JSON、全文搜索
- 插入性能稍慢，但**查询性能极高**
- 使用场景：

  - `@>` 操作符：`WHERE tags @> ARRAY['sql']`
  - JSON 查询：`WHERE data @> '{"name": "Tom"}'`

---

### 4. **如何选择合适的索引类型？**

选择索引类型要根据字段的数据类型和查询方式来决定：

| 需求                       | 推荐索引类型     |
| -------------------------- | ---------------- |
| 等值查询                   | B-Tree、Hash     |
| 范围查询（<、>、BETWEEN）  | B-Tree、GiST     |
| 模糊匹配（`LIKE '%abc%'`） | GIN + trigram    |
| JSON、数组字段             | GIN              |
| 地理数据                   | GiST（PostGIS）  |
| 多值字段（如 `tags[]`）    | GIN              |
| 覆盖索引                   | B-Tree + INCLUDE |

---

### 5. **MySQL 和 PostgreSQL 是否支持部分索引（Partial Index）？**

**部分索引**：只为满足特定条件的数据建立索引，提高效率、减少空间浪费。

#### PostgreSQL 示例：

```sql
CREATE INDEX idx_active_users ON users(name) WHERE active = true;
```

- 只有 `active = true` 的行会出现在索引中，适合高稀疏性字段。

#### MySQL：

- 不支持原生部分索引，但可以借助**前缀索引**模拟部分效果（限制字符串长度）：

```sql
CREATE INDEX idx_name_prefix ON users(name(10)); -- 只索引前10个字符
```

✅ PostgreSQL 的部分索引更灵活，适合有很多 NULL 或特定状态数据的场景。

---

这部分索引相关内容就讲到这里。下一类是：

> 三、事务与锁机制（MVCC、事务隔离、行锁 vs 表锁、死锁检测等）

是否继续讲解这部分？

---

好的，继续讲解第三类：**事务与锁机制**。

---

## **三、事务与锁机制**

事务的核心作用是**保证数据一致性与并发安全性**，理解 MVCC 和锁机制是高质量数据库设计和调优的关键。

---

### 1. **PostgreSQL 的 MVCC（多版本并发控制）是如何实现的？**

**MVCC（Multi-Version Concurrency Control）**：允许多个事务同时读取数据库而互不干扰。

#### PostgreSQL 的实现方式：

- 每一行数据都有两个隐藏字段：`xmin`（插入该行的事务 ID）和 `xmax`（删除该行的事务 ID）。
- 每个查询都会读取**在自己事务视角下可见的版本**，通过比较事务 ID 判断：

  - 当前事务 ID > xmin 且（xmax 不存在或当前事务 ID < xmax）⇒ 可见

- 删除其实是**标记行过期，不会立即删除**，由 `VACUUM` 过程清理

#### 优点：

- 不加锁即可实现一致性读取，性能好
- 避免了读写冲突（读不阻塞写、写不阻塞读）

---

### 2. **MySQL 的 InnoDB 引擎是如何实现 MVCC 的？**

与 PostgreSQL 类似，InnoDB 使用隐藏字段记录版本：

- 两个字段：

  - `DB_TRX_ID`：行最后被修改的事务 ID
  - `DB_ROLL_PTR`：指向 undo log（可以还原出旧版本）

- 在查询时使用 `Read View` 判断哪些版本对当前事务可见

#### 差异：

- PostgreSQL 的 MVCC 是物理多版本（旧版本共存）
- MySQL 的 MVCC 是逻辑版本（使用 undo 日志构造历史版本）

---

### 3. **MySQL 和 PostgreSQL 的行锁、表锁如何使用？**

#### MySQL（InnoDB）支持：

- **行锁**（默认）：通过主键或唯一索引定位 ⇒ 行锁生效
- **表锁**：如 `LOCK TABLES` 显式加锁
- 若 WHERE 无索引或范围不明确 ⇒ 升级为表锁或间隙锁（Gap Lock）

#### PostgreSQL 支持：

- 所有行级操作默认使用**行级锁**
- 显式加锁方式：

  - `SELECT ... FOR UPDATE`
  - `LOCK TABLE table_name IN ACCESS EXCLUSIVE MODE`

---

### 4. **什么是幻读？PostgreSQL 如何避免？**

**幻读**：一个事务中两次执行相同查询，第二次返回了“新插入”的行。

示例：

```sql
-- Tx1:
SELECT * FROM orders WHERE amount > 100;

-- Tx2 插入满足条件的记录:
INSERT INTO orders(amount) VALUES (200);

-- Tx1 再查一次，发现结果增加 ⇒ 幻读
```

#### PostgreSQL 的应对：

- 默认隔离级别是 **Read Committed**，存在幻读
- **Serializable** 隔离级别通过**序列化快照 + 可串行化验证机制**彻底避免幻读（性能开销较高）

---

### 5. **PostgreSQL 如何处理死锁？如何检测和避免？**

#### 死锁产生：

两个事务同时持有锁，并且互相等待对方释放，永远阻塞。

#### PostgreSQL 死锁检测机制：

- 定期扫描锁图，发现循环等待 ⇒ **自动终止其中一个事务**并报错
- 错误示例：

  ```
  ERROR: deadlock detected
  DETAIL: Process 1234 waits for ShareLock on transaction 5678; blocked by process 5678.
  ```

#### 避免死锁建议：

- 尽量**按固定顺序访问资源**
- 减少事务范围，及时提交
- 避免用户长时间交互中持有锁
- 复杂业务中使用 `SELECT ... FOR UPDATE` 控制粒度

---

### ⚠️ 总结对比图：MySQL vs PostgreSQL（事务 & 锁）

| 特性           | MySQL InnoDB                         | PostgreSQL                            |
| -------------- | ------------------------------------ | ------------------------------------- |
| MVCC           | 使用 undo log 生成旧版本             | 每行保留多个版本                      |
| 幻读           | Repeatable Read 已防止（使用间隙锁） | 默认 Read Committed 可能出现幻读      |
| 锁粒度         | 行锁、表锁、间隙锁                   | 行锁、表锁                            |
| 死锁检测       | 自动检测，报错回滚事务               | 自动检测，报错回滚事务                |
| 串行化事务实现 | 加锁机制 + Next-Key Lock             | Serializable Snapshot Isolation (SSI) |

---

这部分就讲完了。

下一类是：

> 四、SQL 编写与优化（Explain 使用、慢查询、CTE、窗口函数等）

是否继续？

---

好的，我们继续第四类：**SQL 编写与优化**

---

## **四、SQL 编写与优化**

---

### 1. **PostgreSQL 和 MySQL 的 SQL 语法有何不同？**

尽管都遵循 SQL 标准，但细节有很多差异：

| 语法项        | MySQL                                | PostgreSQL                             |     |     |
| ------------- | ------------------------------------ | -------------------------------------- | --- | --- |
| `LIMIT`       | `LIMIT 10 OFFSET 5`                  | 一样支持                               |     |     |
| 字符串拼接    | `CONCAT(a, b)`                       | \`a                                    |     | b\` |
| `IF` / `CASE` | `IF(expr, a, b)`                     | 标准 `CASE WHEN THEN`                  |     |     |
| 日期函数      | `DATE_ADD`, `DATE_SUB`               | `interval`, `now() + interval '1 day'` |     |     |
| 随机函数      | `RAND()`                             | `RANDOM()`                             |     |     |
| upsert        | `INSERT ... ON DUPLICATE KEY UPDATE` | `INSERT ... ON CONFLICT DO UPDATE`     |     |     |
| JSON 查询     | `JSON_EXTRACT(json, '$.key')`        | `json->'key'`, `jsonb->>'key'`         |     |     |
| 正则表达式    | `REGEXP`                             | `~`, `~*`, `!~`                        |     |     |

✅ PostgreSQL 更偏向 SQL 标准，MySQL 在兼容性上更宽松。

---

### 2. **Explain 分析如何使用？如何查看 SQL 性能？**

#### MySQL：

```sql
EXPLAIN SELECT * FROM users WHERE name = 'Tom';
```

返回字段：

- `type`：访问类型（ALL、index、range、ref、eq_ref、const、system）
- `possible_keys` / `key`：可能用到的索引、实际使用的索引
- `rows`：扫描的行数
- `Extra`：是否使用了临时表、排序等

#### PostgreSQL：

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE age > 25;
```

返回字段：

- `Seq Scan`, `Index Scan`, `Bitmap Heap Scan` 等不同扫描方式
- `cost`, `rows`, `width`
- 实际执行时间（只有 `ANALYZE` 才有）

✅ PostgreSQL 的 `EXPLAIN ANALYZE` 提供了更详细的实际执行信息，是优化的重要工具。

---

### 3. **PostgreSQL 的 CTE（WITH 语句） 和递归查询使用方法？**

#### 普通 CTE：

```sql
WITH recent_users AS (
  SELECT * FROM users WHERE created_at > now() - interval '7 days'
)
SELECT * FROM recent_users WHERE active = true;
```

#### 递归 CTE（如获取所有下属）：

```sql
WITH RECURSIVE subordinates AS (
  SELECT id, name FROM employees WHERE manager_id IS NULL
  UNION
  SELECT e.id, e.name FROM employees e
  JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates;
```

✅ PostgreSQL 支持强大的 CTE 和递归能力，MySQL 8+ 才开始支持 `WITH`。

---

### 4. **MySQL 如何优化慢查询？**

#### 步骤：

1. **开启慢查询日志**

```ini
slow_query_log = 1
slow_query_log_file = /var/log/mysql-slow.log
long_query_time = 1  -- 超过1秒记录
```

2. **查看慢查询**

```bash
mysqldumpslow /var/log/mysql-slow.log
```

3. **使用 `EXPLAIN` 分析查询计划**

4. **常见优化手段**：

- 添加合适的索引
- 避免 `SELECT *`
- 用 `LIMIT` 限制结果
- 尽量用 `JOIN` 替代子查询
- 避免函数作用于索引字段：`WHERE YEAR(created_at) = 2023` 会失效索引

---

### 5. **PostgreSQL 的窗口函数有哪些使用场景？**

窗口函数是 PostgreSQL 的强大特性之一：

#### 示例语法：

```sql
SELECT id, name, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank
FROM employees;
```

#### 常用窗口函数：

| 函数                       | 含义                  |
| -------------------------- | --------------------- |
| `ROW_NUMBER()`             | 行号                  |
| `RANK()` / `DENSE_RANK()`  | 排名（有并列/无并列） |
| `LAG()`, `LEAD()`          | 上一行、下一行的值    |
| `SUM() OVER`, `AVG() OVER` | 累加、移动平均        |

#### 适用场景：

- 实现分页编号
- 分组内排名
- 比较当前行和前一行数据（如增长率）
- 取每组前 N 条数据（结合 `ROW_NUMBER()`）

✅ PostgreSQL 的窗口函数不仅比 MySQL 更强，而且配合 CTE 使用可以简洁实现复杂分析任务。

---

### ✅ 本节总结

| 特性             | MySQL                  | PostgreSQL                                  |
| ---------------- | ---------------------- | ------------------------------------------- |
| Explain 分析能力 | 较简单                 | 丰富详细（含执行时间）                      |
| CTE              | 8.0 才支持             | 完全支持，递归更强                          |
| 慢查询日志       | 配置开启，常见调优工具 | 使用 `auto_explain` 和 `pg_stat_statements` |
| 窗口函数         | 基本支持               | 原生支持，功能丰富                          |
| 查询优化空间     | 基础查询优化足够       | 高级查询分析非常强大                        |

---

接下来是：

> 五、高级功能和特性（JSON 支持、全文搜索、函数与过程、复制等）

是否继续？

---

我们继续第五类：**高级功能和特性**

---

## **五、高级功能和特性**

这一部分涵盖了 MySQL 和 PostgreSQL 在 JSON 支持、全文搜索、自定义函数、复制等高级能力上的区别与应用。

---

### 1. **MySQL 和 PostgreSQL 对 JSON 的支持有什么不同？**

| 特性       | MySQL                                  | PostgreSQL                                   |
| ---------- | -------------------------------------- | -------------------------------------------- |
| JSON 类型  | `JSON` 类型（5.7+）                    | `JSON`、`JSONB`（更高效）                    |
| 索引支持   | 无法直接对 JSON 字段索引（只能虚拟列） | `JSONB` 可使用 **GIN 索引**                  |
| 操作符     | `JSON_EXTRACT(json, '$.key')`          | `->`, `->>`, `@>`, `#>>`, `jsonb_set()` 等   |
| 函数丰富度 | 较有限                                 | 非常丰富，支持合并、删除、键判断、数组操作等 |
| 查询效率   | JSON 字符串形式处理，查询较慢          | JSONB 二进制存储，**更快更强大**             |

#### PostgreSQL 示例：

```sql
SELECT data->>'name' FROM users WHERE data @> '{"age": 30}';
```

✅ PostgreSQL 是目前结构化数据库中对 JSON 支持最强的系统。

---

### 2. **PostgreSQL 的存储过程与函数有什么区别？**

| 项目             | 函数（FUNCTION）                 | 存储过程（PROCEDURE）                   |
| ---------------- | -------------------------------- | --------------------------------------- |
| 是否有返回值     | 有返回值                         | 无返回值（或通过 `OUT` 参数）           |
| 是否可嵌套调用   | 可以嵌套                         | 不能嵌套在 SELECT 中                    |
| 是否支持事务控制 | 不支持在内部控制事务（自动提交） | ✅ 可使用 `BEGIN`, `COMMIT`, `ROLLBACK` |
| 调用方式         | `SELECT func_name()`             | `CALL proc_name()`                      |
| 使用场景         | 计算、查询、表达式中嵌入         | 复杂业务逻辑、批处理、事务控制          |

---

### 3. **PostgreSQL 如何实现全文搜索？与 ElasticSearch 有何不同？**

#### 内置全文搜索功能：

- 支持词干提取（stemming）、去除停用词（stop words）、语言解析
- 支持 `tsvector`, `tsquery`

#### 示例：

```sql
-- 创建全文索引
CREATE INDEX idx_content ON articles USING GIN(to_tsvector('english', content));

-- 查询匹配关键词
SELECT * FROM articles
WHERE to_tsvector('english', content) @@ to_tsquery('open & source');
```

#### 与 ElasticSearch 比较：

| 特性     | PostgreSQL         | ElasticSearch                    |
| -------- | ------------------ | -------------------------------- |
| 一致性   | 强一致性           | 最终一致性                       |
| 架构     | 单体数据库内嵌     | 分布式搜索引擎                   |
| 实时性   | 实时性弱一些       | 实时搜索能力强                   |
| 查询语言 | SQL 风格           | DSL 风格                         |
| 使用场景 | 适合中小型全文需求 | 大规模、模糊搜索、中文分词等场景 |

✅ PostgreSQL 是轻量级替代 Elasticsearch 的选择之一。

---

### 4. **MySQL 支持的主从复制和读写分离机制？**

#### MySQL 支持三种复制方式：

1. **异步复制（默认）**：主库不等从库确认就提交
2. **半同步复制**：至少一个从库确认后主库才提交
3. **组复制（Group Replication）**：MySQL 5.7+，分布式强一致性

#### 实现读写分离：

- 应用层代码配置主从读写路由
- 或使用中间件：**ProxySQL、MyCat、Atlas**

---

### 5. **PostgreSQL 的外部数据封装器（FDW）是什么？能干什么？**

**FDW（Foreign Data Wrapper）** 是 PostgreSQL 的一项扩展功能，可以让它查询外部数据源。

#### 支持的数据源：

- 其他 PostgreSQL 数据库（postgres_fdw）
- MySQL、MongoDB、Oracle、CSV 文件、REST API 等

#### 示例：

```sql
CREATE EXTENSION postgres_fdw;
CREATE SERVER foreign_srv FOREIGN DATA WRAPPER postgres_fdw
  OPTIONS (host 'remote_host', dbname 'foreign_db', port '5432');

CREATE USER MAPPING FOR current_user SERVER foreign_srv
  OPTIONS (user 'remote_user', password 'pwd');

IMPORT FOREIGN SCHEMA public FROM SERVER foreign_srv INTO local_schema;
```

✅ PostgreSQL 能将“外部表”当成本地表查询，非常适合做数据聚合、联邦查询。

---

### 6. **如何实现 PostgreSQL 的逻辑复制和物理复制？**

#### 物理复制：

- 基于 WAL 日志（二进制日志）
- 文件级同步 ⇒ 用于主从同步、热备份
- `streaming replication` 是主流方式

#### 逻辑复制（PostgreSQL 10+）：

- 支持**按表级别选择复制数据**
- 支持双向复制、跨版本复制、跨平台复制
- 使用 `pgoutput` 插件和 `publication/subscription`

#### 示例：

```sql
-- 主库
CREATE PUBLICATION my_pub FOR TABLE users;

-- 从库
CREATE SUBSCRIPTION my_sub
  CONNECTION 'host=host user=postgres dbname=mydb'
  PUBLICATION my_pub;
```

✅ 逻辑复制适合做 **数据分发、异地复制、分布式扩展**。

---

### 本节小结：

| 高级能力        | MySQL                     | PostgreSQL              |
| --------------- | ------------------------- | ----------------------- |
| JSON 查询       | 有支持但有限              | JSONB + 丰富函数与索引  |
| 存储过程        | 支持                      | ✅ 支持函数和事务型过程 |
| 全文搜索        | 基础支持（match against） | ✅ 支持向量搜索、词根等 |
| 外部数据查询    | 不支持                    | ✅ FDW 支持异构数据源   |
| 逻辑复制        | 无（需第三方工具）        | ✅ 原生支持             |
| GIN / GiST 索引 | ❌                        | ✅                      |

---

接下来是最后一类：

> 六、实践与运维（备份恢复、高可用、连接池、Vacuum、慢查询分析等）

是否继续？
