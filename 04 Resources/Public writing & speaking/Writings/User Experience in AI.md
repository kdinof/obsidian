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
С появлением AI мы переходим от жестких и ограниченных систем (нажал кнопку → произошло действие) к системам, которые учатся, прогнозируют, подстраиваются под пользователя. 
Проще говоря раньше интерфейсы были предсказуемым, а сейчас стали умными, но не всегда предсказуемыми.

Системы стали умнее и лучше адаптироваться под пользователя с одной стороны. Но с другой стороны настороженность к таким системам возрастает особенно, если они работают, как «черный ящик»

Если пользователю понятно, как работает AI, он будет доверять ему. 

Есть несколько принципов в UX, которые помогают завоевать доверие пользователя и решать его задачи быстрее. 

## Прозрачность (Transparency) и Объяснимость (Explainability XAI)
Нужно обеспечить видимость и понятность работы AI. Пользователь должен понимать что именно делает система, на каких данных она основывает решения и почему предложила конкретный результат.

**Прозрачность** — это когда интерфейс показывает «кухню» ИИ: например, помечает, где именно сработал алгоритм, какие данные были использованы, или почему пользователь видит тот или иной совет.
На что важно обратить внимание:
- Пользователь должен понимать, что взаимодействует с AI
- Должно быть понятно, какую информацию LLM использует, по какому плану двигается. 
	- Например, какими данными агент руководствовался, чтобы выдать ответ и через какие шаги прошел. Тут очень хороший пример Deep Research
- В итоге делать процесс работы с AI видимым и прозрачным. 

**Объяснимость (XAI)** идёт дальше: она не просто показывает процесс, а **помогает человеку осознать логику** — _почему_ ИИ выбрал именно этот вариант, какие факторы повлияли на решение, насколько он уверен в результате.
На что тут важно обратить внимание:
- Когда показывать диалог LLM с самим собой
- Давать LLM взаимодействовать с пользователем. 

> 💬 _Ключевая цель — снизить чувство “чёрного ящика” и создать ощущение, что ИИ — это партнёр, а не непонятная машина, принимающая решения за пользователя._

## Контроль пользователя (User Control / Adjustable Autonomy)
Пользователь должен чувствовать, что **он управляет системой, а не наоборот** — что ИИ выступает в роли соавтора, партнёра по процессу, а не автономного исполнителя. Контроль по-прежнему остаётся у человека

Человек должен уметь влиять на автономию и выбирать ее уровень. Например
— когда ИИ действует сам,  
— когда предлагает варианты,  
— а когда просто подсказывает.
(очень хороший пример Cursor или Claude Code)

Кроме выбора уровня автономии, пользователь должен иметь **инструменты управления самим агентом**:
- **Направлять и гайдить** — задавать чёткие шаги или стратегию работы, утверждать решения.
- **Исправлять результаты (refine)** — уточнять, корректировать, дорабатывать предложенное.
- **Отменять или останавливать (override)** — прерывать нежелательные действия.
- **Возвращать систему в предыдущее состояние (reverse)** — откатывать шаги ИИ, если результат не удовлетворяет.


## Feedback Loops & Learning
ИИ-продукты должны не просто реагировать на действия пользователя, а **учиться у него** — превращать каждое взаимодействие в сигнал для улучшения.  
Это и есть **петля обратной связи**: когда система адаптируется, исходя из того, что человек принимает, редактирует или отклоняет.

Как система может учится: 
- **Level 1: Micro Feedback (UI Layer)** Обратная связь происходит на уровне интерфейса — это моментальные реакции: “лайк”, “дизлайк”, “undo”, “regenerate”.
	- Пользователь чувствует, что может **влиять на результат прямо здесь и сейчас**, но система не делает долговременных выводов.
	- Пример: кнопка 👍/👎 в ChatGPT, «Не показывать это видео» на YouTube.
- **Level 2: Interaction Feedback (Task Layer)** ИИ начинает **связывать обратную связь с конкретной задачей или контекстом**. Он анализирует цепочку действий — корректировки текста, изменения параметров, выбор альтернатив — и использует это для улучшения результата в рамках текущего сеанса или задачи.
- **Level 3: Behavior Feedback (Session Layer)** Система уже **помнит и адаптируется к индивидуальному стилю**. 
	- Это уровень внутренней “памяти” — ИИ накапливает знания о предпочтениях пользователя и применяет их в будущем взаимодействии.
	- Пример: внутренняя память ChatGPT, персонализация в Notion AI или Midjourney.

## «Невидимый интерфейс» и Capability Awareness в AI-продуктах
В классическом софте управление видно: кнопки, меню, горячие клавиши — явные **affordances**.  
В открытых AI-интерфейсах (поле ввода + «Сгенерировать») **возможности скрыты**: пользователь не знает, _что вообще умеет система, как к этому обратиться и как «правильно» попросить_.

Отсюда три симптома (ты их уже точно подметил):
- **Нет подсказок**: просто инпут → «и что сюда писать?»
- **Слишком много возможностей**: выбор парализует, «с чего начать?»
- **Падающая эффективность**: разные формулировки → разные результаты, без направляющих качество плавает.

Почему так происходит (корень)
- **Переход к вероятностному поведению**: система «предполагает лучший ответ», а не исполняет фиксированную команду.
- **Ментальные модели не совпадают**: человек ожидает инструмент, а получает «собеседника».
	- Тут очень интересно, как это можно спроецировать на банковские сервисы. Нужен ли вообще инструмент или собеседник.
- **Нехватка явных affordances** в open-ended UI: нет меню действий, нет «режимов», нет ограничений, которые подсказывают границы возможностей.

### Паттерны решения: от интерфейса к поведению
- UI-уровень (сделать возможности _видимыми_)
	- **Многошаговый “пустой экран”**
		- Чипы-примеров по _целям_ (а не по синтаксису): «Переписать тон строже», «Разбить на задачи», «Сгенерировать тесты».
		- 3–5 «карточек возможностей» (Capability Cards) с короткой формулой запроса и результатом до/после.
	- **Командная палитра / “/”-меню**
		- Ввод «/» раскрывает список навыков: _/summarize, /critique, /refactor, /diagram_.
		- Динамика по контексту (выделенный текст, тип файла, роль пользователя).
	- **Контекстная панель действий**
		- Выделяешь объект → появляются релевантные AI-кнопки (Explain, Simplify, Translate, Create Tests).
		- Убирает нужду «придумывать промпт».
	- **Слот-билдер (Prompt Builder)**
		- Вместо «напиши идеальный промпт» — форма из полей: _Цель → Стиль → Ограничения → Пример входа → Формат выхода_.
		- Результат прозрачен, пользователь учится на структуре.
- Task-уровень (сценарное «окаймление» возможностей)
- Behavior/Memory-уровень (учиться на человеке и показывать это)

## Как это выглядит в реальных продуктах (ориентиры)=
- **Cursor / Claude Code** — сильная _Task-и UI-сцепка_: команды, шаблоны, быстрые правки, видимые режимы генерации/редакции.
- **Figma AI Assist** — контекстные действия из выделения (affordances прямо в холсте).
- **Notion AI** — режимы + слот-билдеры; хорошие «пустые экраны» с примерами по задачам.
- **Grammarly** — ясная зона применения + объяснимые правки (что и почему).

---











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
- Направлять и гайдить. Давать, аппрувить четкие механизмы, шаги работы AI 
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

| Stage | Name            | Description                                                         | Example                                       |
| ----- | --------------- | ------------------------------------------------------------------- | --------------------------------------------- |
| **1** | _Reactive_      | Users correct errors manually; AI doesn’t adapt.                    | Early chatbots, static forms.                 |
| **2** | _Responsive_    | AI logs corrections; adapts slightly.                               | Gmail Smart Reply.                            |
| **3** | _Adaptive_      | AI learns personal patterns; feedback visible.                      | Netflix, Grammarly, Duolingo.                 |
| **4** | _Collaborative_ | User & AI co-evolve shared understanding; AI explains its learning. | Figma AI Assist, ChatGPT iterative prompting. |
| **5** | _Reflexive_     | AI learns from _human systems_ (communities, ethics reviews).       | Wikipedia moderation AI, Tesla fleet data.    |


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