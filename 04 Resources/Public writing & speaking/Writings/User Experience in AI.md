---
title_en: User Experience in AI
title_ru: Пользовательский опыт в UX
tags:
  - UX
  - ai
  - user-experience
language is RU: true
type:
  - post
status:
  - idea
date-published:
---
https://chatgpt.com/share/68e78fe0-ba94-8013-9b73-0f613e979060

Основная мысль начинается с того, что интерфейсы сейчас меняются от детерминированных к адаптивным. То есть интерфейс продукта может подстраиваться. 

LLM и AI продукты становятся операционными системами для других продуктов, которые дают ценность пользователя. Например Open AI открыли SDK для разработки приложений, которые работают внутри ChatGPT. 
Можно подробно ознакомиться с гайдлайнами https://developers.openai.com/apps-sdk/concepts/design-guidelines

То есть, ещё раз. Меняется не только процесс, того, как мы разрабатываем интерфейсы, но и то, каким принципами интерфейс следует. И тут я хочу чуть поглубже нырнуть в эти самые принципы. 

Вообще принципы работы с AI раскрывают одну важную мысль: «у руля все ещё стоит человек, а AI ассистирует ему и как хорошим ассистентом AI можно управлять, перенаправлять, наблюдать за процессом»

Всего принципов 3: 
- Прозрачность
- Объяснимость 
- Контроль

## Прозрачность (Transparency)
Отвечает за то, чтобы обеспечить видимость и понятность работы с AI. Тут я специально не пишу «работы AI», а именно «работы С ai» т.к. выходит далеко за пределы интерфейса. 

На что важно обращать внимание:
- Пользователь должен понимать, что взаимодействует с AI. 
	- Информирование, что в процесс вмешивается агент / реальный человек
	- Подсветка контента, которые сделан с помощью AI
- Раскрывать информацию об использовании данных, логики шагов. Например, какими данными агент руководствовался, чтобы выдать ответ и через какие шаги прошел. Тут очень хороший пример Deep Research
- В итоге делать процесс работы AI видимым и понятным. 

## Объяснимость Explainability XAI
Ох, тут сейчас будет сложно. Короче, объяснимость (Explainability) идет прям очень близко к (Transparency). Объяснимость отвечает на вопрос «почему» и больше относиться к процессу работы AI (почему это только что произошло). Прозрачность отвечает на вопрос «как» и больше относиться к процессу работы С AI (как получился вот такой ответ). 
Давайте пример с операционной системой. Вот вы поставили zoom и хотите сделать первый звонок. Zoom попросит доступ к микрофону, расскажет, зачем это (прозрачность). Но в момент, когда у вас что-то сломается или вы выключили микрофон и говорите Zoom подсветит вам это. 

Объяснимость выходит за рамки простой прозрачности (которая показывает, как работает ИИ) и фокусируется на объяснении того, как и почему ИИ пришел к определенному результату или принял конкретное решение

В UX паттернах это может проявлятся через
- диалог агента с самим собой
- диалог LLM / агента с пользователем
- Интерактивные элементы, подсветка

## Control & Override / Human-in-the-Loop
Тут явно раскрывается основной принцип, что у руля все ещё стоит человек, а не AI. Пользователь является активным соавтором. 

Пользователь должен уметь
- Направлять и гайдить. Давать, аппрувить четкие механизмы, шани работы AI 
- Исправлять результаты работы (refine), 
- Отменять, или останавливать (override) 
- и даже обратного действия (reverse) ИИ-действий

## Feedback Loops & Learning
Принцип самоусиливающейся системы. Чем сильнее и дольше связь пользователя и AI, тем умнее и более согласованными становятся AI агенты

Как обучаются агенты 
- Level 1: Micro Feedback (UI Layer). Обучение в рабочем процессе (Learning within the Workflow) Лайки, или в GPR выбрать лучший вариант
- Level 2: Interaction Feedback (Task Layer). GitHub Copilot adapts to your coding style when you reject or modify its completions. ChatGPT “Regenerate / Edit / Rate” buttons teach it better response calibration
	- ### 🧩 **2. Grammarly: Interaction Feedback**
	- **User action:** Accept or ignore grammar suggestions.
	- **System learning:** Adapts its tone and style recommendations to your writing behavior over time.
	- **HCAI value:** _Co-adaptation_ — it learns your personal writing norms rather than enforcing a rigid grammar model.
	- 💬 The “Tone Detector” becomes more accurate the more you interact with it — a living example of _personalized explainability._ 
- Level 3: Behavior Feedback (Session Layer). Внутренняя память chatGPT.
	- 🧩 3. Figma’s “AI Design Assist” (experimental tools)
	- User action: Adjust generated layouts, reject certain style recommendations.
	- System learning: Refines next suggestions using design structure feedback.
	- HCAI value: Keeps the designer in the loop — human taste becomes part of the model’s ongoing tuning.

|Stage|Name|Description|Example|
|---|---|---|---|
|**1**|_Reactive_|Users correct errors manually; AI doesn’t adapt.|Early chatbots, static forms.|
|**2**|_Responsive_|AI logs corrections; adapts slightly.|Gmail Smart Reply.|
|**3**|_Adaptive_|AI learns personal patterns; feedback visible.|Netflix, Grammarly, Duolingo.|
|**4**|_Collaborative_|User & AI co-evolve shared understanding; AI explains its learning.|Figma AI Assist, ChatGPT iterative prompting.|
|**5**|_Reflexive_|AI learns from _human systems_ (communities, ethics reviews).|Wikipedia moderation AI, Tesla fleet data.


  ---
**Все уровни**

|Trust Lever|Description / What to design for|Why it matters / example|
|---|---|---|
|**Transparency / Explainability**|Show how the AI reached its conclusions (in user-friendly terms) — e.g. “We recommended this because X, Y, Z.” Possibly with visual or conversational explanations.|If AI is a black box, users can’t reason about failures or trust it. Even imperfect models feel more trustworthy when you see the logic behind them. ([Medium](https://medium.com/design-bootcamp/how-to-actually-design-a-human-centered-ai-mvp-720ca6e229d2?utm_source=chatgpt.com "How To Actually Design A Human-Centered AI MVP"))|
|**Control & Override / Human-in-the-Loop**|Let users intervene, override, correct, or guide the model. Provide scaffolding (e.g. “Would you like to adjust this suggestion?”)|Gives users agency and sense of safety: they’re not just passive consumers of AI. ([arXiv](https://arxiv.org/pdf/2002.04087?utm_source=chatgpt.com "Human-Centered Artificial Intelligence: Reliable, Safe & ..."))|
|**Feedback Loops & Learning**|Embed micro-feedback (e.g. thumbs up/down, “Was this helpful?”) and let the system adapt to user preferences over time|Reinforces the idea that the system is responsive, personalizes better, and that the user’s input matters. ([Medium](https://medium.com/design-bootcamp/how-to-actually-design-a-human-centered-ai-mvp-720ca6e229d2?utm_source=chatgpt.com "How To Actually Design A Human-Centered AI MVP"))|
|**Fairness, Bias Mitigation & Inclusivity**|Design to avoid discriminatory or unfair behavior, be sensitive to different groups’ needs, evaluate across demographics|If users see bias or unfairness, trust breaks immediately.|
|**Reliability, Safety, Robustness**|The AI should behave predictably, gracefully degrade under uncertainty, handle edge cases, fail safely|In critical domains (medicine, safety, finance), a single failure can kill trust. HCAI emphasizes _Reliable, Safe & Trustworthy_ systems. ([arXiv](https://arxiv.org/pdf/2002.04087?utm_source=chatgpt.com "Human-Centered Artificial Intelligence: Reliable, Safe & ..."))|
|**Privacy & Security**|Be transparent about data use, limit data collection, anonymize or encrypt, get user consent|Because data misuse is one of the biggest trust breakdowns in AI systems|
|**User Involvement & Co-Design**|Involve real users in design, testing, prototyping phases. Make them part of AI definition.|Helps prevent mismatches between what users expect and what AI does; builds psychological ownership|
|**Empathy & Tone (UX around AI)**|Use language, visuals, and interactions that communicate humility, uncertainty, and human understanding (not overconfident “I know best”)|Overconfidence or arrogance in AI voice can erode trust; acknowledging “I’m uncertain” humanizes the system.|
|**Calibration of Trust**|Help users calibrate their trust correctly — neither overtrust (blindly accept) nor undertrust (never use)|For example, nudges or disclaimers ("Consider verifying this recommendation") help users maintain critical thinking|





---

Четкое понимание возможностей ИИ (Capability Awareness), или решение проблемы «Невидимого интерфейса» (Invisible UI problem), рассматривается в источниках как самая большая проблема в современных интерфейсах AI-продуктов и является одной из наиболее распространенных Общих проблем AI-продуктов (Common AI Product Issues).
Эта проблема возникает из-за того, что, в отличие от традиционного программного обеспечения с четкими визуальными элементами управления (affordances), открытые (open-ended) интерфейсы к моделям ИИ не дают пользователям понять, что именно они могут делать и как добиться наилучших результатов

Проблема «Невидимого интерфейса»
- Отсутствие подсказок. Зачастуе это просто поле ввода. 
- Слишком много возможностей. это паралич выбора. Пользователю сложно придумать, что именно он может сделатьс AI
- Снижение эффективности.  Разные формулировк — разные результаты. Без четких подсказок 

Как решается на уровне UX
- оптимизация подсказок и улучшения. Например оптимизация промпта. 