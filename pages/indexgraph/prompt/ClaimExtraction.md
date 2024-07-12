
# Claim Extraction 提示词中文版


```python
"""包含提示定义的文件。"""

CLAIM_EXTRACTION_PROMPT = """
-目标活动-
你是一个智能助手，帮助人工分析师分析文本文档中针对特定实体的索赔。

-目标-
给定一个可能与此活动相关的文本文档、一个实体规范和一个索赔描述，提取与实体规范匹配的所有实体以及所有针对这些实体的索赔。

-步骤-
1. 提取与预定义实体规范匹配的所有命名实体。实体规范可以是实体名称列表或实体类型列表。
2. 对于在步骤1中识别的每个实体，提取与该实体相关的所有索赔。索赔需要与指定的索赔描述匹配，并且该实体应为索赔的主题。
对于每个索赔，提取以下信息：
- 主题: 索赔的主题实体的名称，首字母大写。主题实体是在索赔中描述的行为的主体。主题实体应为步骤1中识别的命名实体之一。
- 对象: 索赔的对象实体的名称，首字母大写。对象实体是报告/处理或受到索赔所描述行为影响的实体。如果对象实体未知，则使用 **NONE**。
- 索赔类型: 索赔的整体类别，首字母大写。以一种在多个文本输入中可重复使用的方式进行命名，以便相似的索赔共享相同的索赔类型
- 索赔状态: **TRUE**、**FALSE** 或 **SUSPECTED**。 TRUE 表示确认该索赔，FALSE 表示索赔被证明是错误的，SUSPECTED 表示索赔尚未验证。
- 索赔描述: 详细描述解释索赔背后的推理，以及所有相关证据和参考资料。
- 索赔日期: 索赔发布日期的范围 (start_date, end_date)。start_date 和 end_date 应使用 ISO-8601 格式。如果索赔是根据单个日期而不是日期范围发布的，将同样的日期设置为 start_date 和 end_date。如果日期未知，则返回 **NONE**。
- 索赔源文本: 原始文本中与索赔相关的**所有**引用的列表。

将每个索赔格式化为 (<subject_entity>{tuple_delimiter}<object_entity>{tuple_delimiter}<claim_type>{tuple_delimiter}<claim_status>{tuple_delimiter}<claim_start_date>{tuple_delimiter}<claim_end_date>{tuple_delimiter}<claim_description>{tuple_delimiter}<claim_source>)

3. 以英文形式返回输出，作为步骤1和步骤2中识别的所有索赔的单个列表。使用 **{record_delimiter}** 作为列表分隔符。

4. 完成后，输出 {completion_delimiter}

-示例-
示例 1:
实体规范: 组织
索赔描述: 与实体相关的红旗警示
文本: 根据2022/01/10的一篇文章，公司 A 在参与政府机构 B 发布的多个公开招标时被罚款因串标。该公司为 C 人所拥有，并且该人在2015年涉嫌参与腐败活动。
输出:

(COMPANY A{tuple_delimiter}GOVERNMENT AGENCY B{tuple_delimiter}反竞争行为{tuple_delimiter}TRUE{tuple_delimiter}2022-01-10T00:00:00{tuple_delimiter}2022-01-10T00:00:00{tuple_delimiter}根据2022/01/10发布的一篇文章，公司 A 被发现参与反竞争行为，因为在政府机构 B 发布的多个公开招标中被罚款{tuple_delimiter}根据2022/01/10发布的一篇文章，公司 A 在参与政府机构 B 发布的多个公开招标时被罚款。)
{completion_delimiter}

示例 2:
实体规范: 公司 A，人 C
索赔描述: 与实体相关的红旗警示
文本: 根据2022/01/10的一篇文章，公司 A 在参与政府机构 B 发布的多个公开招标时被罚款因串标。该公司为 C 人所拥有，并且该人在2015年涉嫌参与腐败活动。
输出:

(COMPANY A{tuple_delimiter}GOVERNMENT AGENCY B{tuple_delimiter}反竞争行为{tuple_delimiter}TRUE{tuple_delimiter}2022-01-10T00:00:00{tuple_delimiter}2022-01-10T00:00:00{tuple_delimiter}根据2022/01/10发布的一篇文章，公司 A 被发现参与反竞争行为，因为在政府机构 B 发布的多个公开招标中被罚款{tuple_delimiter}根据2022/01/10发布的一篇文章，公司 A 在参与政府机构 B 发布的多个公开招标时被罚款。)
{record_delimiter}
(PERSON C{tuple_delimiter}NONE{tuple_delimiter}腐败{tuple_delimiter}SUSPECTED{tuple_delimiter}2015-01-01T00:00:00{tuple_delimiter}2015-12-30T00:00:00{tuple_delimiter}C 人在2015年涉嫌参与腐败活动{tuple_delimiter}该公司为 C 人所拥有，并且该人在2015年涉嫌参与腐败活动)
{completion_delimiter}

-真实数据-
请使用以下输入作为你的答案。
实体规范: {entity_specs}
索赔描述: {claim_description}
文本: {input_text}
输出："""
  
  
CONTINUE_PROMPT = "上次提取中遗漏了很多实体。请按照相同的格式在下面添加它们:\n"
LOOP_PROMPT = "似乎有一些实体可能仍然被忽略了。如果还有需要添加的实体，请回答 YES{tuple_delimiter}NO。\n" 

```