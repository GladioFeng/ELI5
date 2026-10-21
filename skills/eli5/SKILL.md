---
name: eli5
description: "Explain concepts, code, errors, and technical passages in plain language, adapting to the reader while preserving accuracy. Use for ELI5, explain simply, break this down, 说人话, 通俗解释, 这是什么意思, or 看不懂 when the user wants to understand a topic. Prefer examples from the current task. Pure prose polishing without an explanation request is outside this skill."
---

# Explain Clearly (ELI5)

Explain the topic at the reader's level, using plain language and concrete examples. Keep the mechanism and any conditions that affect the conclusion accurate.

## Step 1: Identify the Audience

Parse the user's request to determine who the explanation is for. The audience falls into one of these categories:

### Ages
| Audience | Style |
|----------|-------|
| Age 5 | Super simple words. Use fun analogies with toys, animals, candy, playground. Short sentences. "Imagine you have a box of crayons..." |
| Age 10 | Elementary school level. Can handle basic cause-and-effect. Use school, sports, video game analogies. |
| Age 15 | Teenager. Can handle some abstraction. Use social media, phone, gaming references. Be slightly casual. |
| Age 20-30 | Young adult. Clear and direct. Real-world analogies from daily life, work, money. |
| Age 40+ | Mature adult. Respectful tone. Analogies from home ownership, career, family management. |

### Grade / Education Levels
| Audience | Style |
|----------|-------|
| 5th grade | Simple vocabulary, concrete examples, avoid jargon entirely. "Think of it like..." |
| Middle school | Can introduce basic terminology with definitions. Step-by-step logic. |
| Senior High | Can handle moderate complexity. Introduce proper terms but explain them. SAT-level vocabulary OK. |
| College Student | Academic framing. Can use technical terms with brief context. Theory + practical application. |
| Graduate school | Assume strong foundational knowledge. Focus on nuance, trade-offs, edge cases, and deeper implications. Be precise. |

### Job Roles
| Audience | They care about... | Frame explanations around... |
|----------|-------------------|------------------------------|
| Manager | Impact, timeline, risk, cost | Business outcomes, team implications, what decisions need to be made |
| Engineer | How it works, architecture, trade-offs | Technical details, implementation, performance, maintainability |
| Designer | User experience, visual impact, flow | How it affects the user, interaction patterns, accessibility |
| Director | Strategy, ROI, competitive advantage | Big picture, market position, resource allocation |
| Colleague | Practical context, shared work | How it affects their work, what they need to know to collaborate |
| Product Manager | User value, priorities, scope | Feature impact, user stories, what to build vs. skip |

### Relationships
| Audience | Tone | Analogy style |
|----------|------|---------------|
| Wife / Husband / Partner | Warm, conversational, patient | Household tasks, shared experiences, daily routines |
| Father / Mother / Parents | Respectful, clear, no condescension | Familiar technology they use, home analogies, generational bridges |
| Kids / Children | Playful, encouraging, short | Games, cartoons, school, animals |
| Friend | Casual, maybe humorous | Pop culture, shared interests, "you know how..." |

Unless the user explicitly requests another audience, default to an adult with a research background who is unfamiliar with this particular topic. Do not assume that research experience includes AI, statistics, or programming knowledge. Treat the audience tables above as optional guidance for explicitly requested audiences.

Use Chinese by default in a Chinese conversation. Follow an explicit language request; in an English conversation, answer in English. Use a respectful, conversational adult tone without child-directed enthusiasm.

## Step 2: Read the Source Material

Use the supplied passage, current task, and already verified context to understand what needs explaining. Read additional source material only when it is needed to resolve the explanation:
- **Code**: Read the relevant code files. Understand what the code does at a high level before translating.
- **A concept**: Break it into its core components.
- **An error message**: Understand the root cause, not just the surface text.
- **A technical document**: Extract the key points that matter.
- **Anything else**: Identify the essential "what" and "why."

## Step 3: Craft the Explanation

Follow these principles, scaled to the audience:

### Structure
A useful explanation usually starts with a plain-language answer, develops the mechanism through a concrete example, and states any limitation that changes how to interpret it. Use the amount of structure the question needs; a short clarification may need only a paragraph.

- Prefer an example from the current project or domain. For a classification question, use candidate labels and predictions; do not force a story about toys or household objects.
- Introduce unfamiliar terms when they first appear, without defining them through more unfamiliar jargon.
- Use an analogy only if it helps. Explain where it stops matching the actual mechanism when that matters.
- Label invented examples as hypothetical. Do not present example numbers, identities, or outcomes as facts about the user's project.
- Stop when the question is answered. Do not add a compulsory summary, lesson, or follow-up question.

### Language Calibration

For **simple audiences** (young ages, non-technical roles, family):
- Use familiar words. Keep essential technical terms and explain them immediately.
- One idea per sentence.
- Show the concrete action, cause, and result before adding abstraction.
- Use "you" and "your" — make it personal.

For **technical audiences** (engineers, grad students):
- Use terminology the reader already knows, and explain unfamiliar terms; a technical job title does not establish expertise in every topic.
- Focus on the *interesting* parts: trade-offs, edge cases, design decisions.
- Connect to verified prior knowledge only when that connection helps.
- Be concise — respect their existing knowledge.

For **business audiences** (managers, directors):
- Lead with impact and outcomes.
- Quantify using supplied or verified numbers; clearly label hypothetical values and do not invent costs or timelines.
- Skip implementation details unless asked.
- Frame in terms of decisions: "This means we should..."

### Tone Matching
- Ages 5-10: Enthusiastic, like a favorite teacher. "Oh, this is a cool one!"
- Teenagers: Slightly casual but not cringey. No "fellow kids" energy.
- Professionals: Confident and clear. Respect their intelligence while bridging knowledge gaps.
- Family: Patient, warm, conversational. Like explaining over dinner.

## Examples

**User says**: "MLP 容量是什么意思？"
**Audience**: Adult researcher unfamiliar with this concept (default)
**Response style**: "MLP 容量可以理解为这个网络能学多复杂的规律。假设输入相同，把隐藏层从 512 个神经元加到 2048 个，就给了它更多可学习的特征组合。它可能更容易区分类别，但也需要更多计算，数据不足时更容易过拟合。这里的 2048 指隐藏特征数量，输出类别数另由分类头决定。"

**User says**: "mask 后概率为 1，说人话解释一下。"
**Audience**: Adult researcher unfamiliar with this mechanism (default)
**Context**: The question concerns a classifier whose hierarchical mask leaves one known species as the only candidate.
**Response style**: "假设某个属实际有甲、乙两种病毒，但分类头只收录了甲种。模型选中这个属后，mask 把其他属的候选去掉，只剩甲种。softmax 就会把全部概率给它，显示 100%。这个 100% 表示剩余候选里只有甲种，并不证明输入就是甲种。输入即使是未知的乙种，这一步也一样；如果前面的门控放行，就可能分错。"

**User says**: "为什么不能直接降低阈值，把正确答案放出来？"
**Audience**: Adult researcher unfamiliar with selective prediction (default)
**Response style**: "我们事后对照标准答案，才知道被撤回的预测里哪些其实是正确的。模型当时没有标准答案，只能按分数筛选。降低门槛会放出更多正确预测，也可能一起放出更多错误预测。关键是让分数能更好地区分这两类。取消拒判是更极端的设置：如果每条输入都必须报一个病毒类别，背景也会全部被误报；这不等于稍微降低阈值就一定会出现同样的结果。"

## Important Reminders

- Respect the reader's intelligence. Explain the unfamiliar concept without treating the reader as a child unless that audience was explicitly requested.
- When explaining code, explain the purpose before the mechanism; include syntax when it helps answer the question.
- Simplify vocabulary and structure while preserving facts, numbers, conditions, scope, citations, and the strength of the conclusion. Never trade accuracy for readability. Keep qualifications that affect interpretation, distinguish observed results from hypotheses, and say when the available evidence is insufficient.
- Match the depth to the question and the requested audience. Add detail when it resolves confusion, rather than to display expertise.
