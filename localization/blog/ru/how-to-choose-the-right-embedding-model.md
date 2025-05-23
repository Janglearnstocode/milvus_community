---
id: how-to-choose-the-right-embedding-model.md
title: Как выбрать правильную модель встраивания?
author: Lumina Wang
date: 2025-04-09T00:00:00.000Z
desc: >-
  Изучите основные факторы и лучшие практики для выбора правильной модели
  встраивания для эффективного представления данных и повышения
  производительности.
cover: assets.zilliz.com/Complete_Workflow_31b4ac825c.gif
tag: Engineering
tags: >-
  Embedding Model, RAG, Model Selection, Machine Learning, Performance
  Optimization
canonicalUrl: 'https://milvus.io/blog/how-to-choose-the-right-embedding-model.md'
---
<p>Выбор правильной <a href="https://zilliz.com/ai-models">модели встраивания</a> - критически важное решение при создании систем, которые понимают и работают с <a href="https://zilliz.com/learn/introduction-to-unstructured-data">неструктурированными данными</a>, такими как текст, изображения или аудио. Эти модели преобразуют исходные данные в высокоразмерные векторы фиксированного размера, которые передают семантический смысл, что позволяет использовать их для поиска по сходству, рекомендаций, классификации и т. д.</p>
<p>Но не все модели встраивания созданы одинаковыми. При таком количестве вариантов как выбрать правильную? Неправильный выбор может привести к неоптимальной точности, узким местам в производительности или ненужным затратам. В этом руководстве представлена практическая схема, которая поможет вам оценить и выбрать оптимальную модель встраивания для ваших конкретных требований.</p>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/Complete_Workflow_31b4ac825c.gif" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="1-Define-Your-Task-and-Business-Requirements" class="common-anchor-header">1. Определите свою задачу и бизнес-требования<button data-href="#1-Define-Your-Task-and-Business-Requirements" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>Прежде чем выбрать модель встраивания, начните с уточнения основных целей:</p>
<ul>
<li><strong>Тип задачи:</strong> Начните с определения основного приложения, которое вы создаете - семантический поиск, система рекомендаций, конвейер классификации или что-то другое. Для каждого случая использования существуют свои требования к тому, как вкрапления должны представлять и организовывать информацию. Например, если вы создаете систему семантического поиска, вам нужны модели типа Sentence-BERT, которые улавливают нюансы семантического смысла между запросами и документами, обеспечивая близость схожих понятий в векторном пространстве. Для задач классификации вкрапления должны отражать структуру, характерную для конкретной категории, чтобы входы из одного класса располагались в векторном пространстве близко друг к другу. Это облегчает последующим классификаторам различение классов. Обычно используются такие модели, как DistilBERT и RoBERTa. В рекомендательных системах целью является поиск вкраплений, которые отражают отношения между пользователем и элементом или предпочтения. Для этого можно использовать модели, специально обученные на данных неявных отзывов, например нейронную коллаборативную фильтрацию (NCF).</li>
<li><strong>Оценка рентабельности инвестиций:</strong> Сопоставьте производительность и затраты с учетом специфики вашего бизнеса. Критически важные приложения (например, диагностика в здравоохранении) могут оправдать использование моделей премиум-класса с более высокой точностью, поскольку это может быть вопросом жизни и смерти, в то время как чувствительные к затратам приложения с большим объемом требуют тщательного анализа соотношения цены и качества. Главное - определить, оправдывает ли повышение производительности всего на 2-3 % потенциально значительное увеличение затрат в вашем конкретном сценарии.</li>
<li><strong>Другие ограничения:</strong> Учитывайте свои технические требования при отборе вариантов. Если вам нужна многоязычная поддержка, многие общие модели не справляются с неанглийским контентом, поэтому могут потребоваться специализированные многоязычные модели. Если вы работаете в специализированных областях (медицина/юриспруденция), встраиваемые модели общего назначения часто не понимают специфический жаргон - например, они могут не понимать, что <em>"stat"</em> в медицинском контексте означает <em>"немедленно",</em> или что <em>"consideration"</em> в юридических документах означает нечто ценное, обмененное в контракте. Аналогичным образом, аппаратные ограничения и требования к задержкам напрямую влияют на то, какие модели могут быть использованы в вашей среде развертывания.</li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/clarify_task_and_business_requirement_b1bce2ccc0.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="2-Evaluate-Your-Data" class="common-anchor-header">2. Оцените свои данные<button data-href="#2-Evaluate-Your-Data" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>Характер ваших данных существенно влияет на выбор модели встраивания. Ключевыми факторами являются:</p>
<ul>
<li><strong>Модальность данных:</strong> Являются ли ваши данные текстовыми, визуальными или мультимодальными по своей природе? Подберите модель в соответствии с типом данных. Используйте модели на основе трансформаторов, такие как <a href="https://zilliz.com/learn/what-is-bert">BERT</a> или <a href="https://zilliz.com/learn/Sentence-Transformers-for-Long-Form-Text">Sentence-BERT</a> для текста, <a href="https://zilliz.com/glossary/convolutional-neural-network">архитектуры CNN</a> или Vision Transformers<a href="https://zilliz.com/learn/understanding-vision-transformers-vit">(ViT</a>) для изображений, специализированные модели для аудио, а также мультимодальные модели, такие как <a href="https://zilliz.com/learn/exploring-openai-clip-the-future-of-multimodal-ai-learning">CLIP</a> и MagicLens, для мультимодальных приложений.</li>
<li><strong>Специфика домена:</strong> Подумайте, достаточно ли общих моделей, или вам нужны модели, ориентированные на конкретную область, которые понимают специализированные знания. Общие модели, обученные на различных наборах данных (например, <a href="https://zilliz.com/ai-models/text-embedding-3-large">модели встраивания текста OpenAI</a>), хорошо работают для общих тем, но часто упускают тонкие различия в специализированных областях. Однако в таких областях, как здравоохранение или юридические услуги, они часто упускают тонкие различия, поэтому вкрапления, специфичные для конкретной области, такие как <a href="https://arxiv.org/abs/1901.08746">BioBERT</a> или <a href="https://arxiv.org/abs/2010.02559">LegalBERT</a>, могут быть более подходящими.</li>
<li><strong>Тип вкрапления:</strong> <a href="https://zilliz.com/learn/sparse-and-dense-embeddings">Разреженные вкрапления</a> отлично справляются с подбором ключевых слов, что делает их идеальными для каталогов продукции или технической документации. Плотные вкрапления лучше передают семантические связи, что делает их подходящими для запросов на естественном языке и понимания намерений. Многие производственные системы, такие как системы рекомендаций для электронной коммерции, выигрывают от гибридного подхода, использующего оба типа, например, используя <a href="https://zilliz.com/learn/mastering-bm25-a-deep-dive-into-the-algorithm-and-application-in-milvus">BM25</a> (разреженные) для сопоставления ключевых слов и добавляя BERT (плотные вкрапления) для улавливания семантического сходства.</li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/evaluate_your_data_6caeeb813e.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="3-Research-Available-Models" class="common-anchor-header">3. Исследование доступных моделей<button data-href="#3-Research-Available-Models" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>После понимания задачи и данных пришло время изучить доступные модели встраивания. Вот как можно подойти к этому вопросу:</p>
<ul>
<li><p><strong>Популярность:</strong> Отдавайте предпочтение моделям с активными сообществами и широким распространением. Такие модели обычно имеют лучшую документацию, более широкую поддержку сообщества и регулярные обновления. Это может значительно снизить трудности при внедрении. Ознакомьтесь с ведущими моделями в вашей области. Например:</p>
<ul>
<li>Для текста: рассмотрите вкрапления OpenAI, варианты Sentence-BERT или модели E5/BGE.</li>
<li>Для изображений: посмотрите на ViT и ResNet, или CLIP и SigLIP для выравнивания текста и изображения.</li>
<li>Для аудио: проверьте PNN, CLAP или <a href="https://zilliz.com/learn/top-10-most-used-embedding-models-for-audio-data">другие популярные модели</a>.</li>
</ul></li>
<li><p><strong>Авторское право и лицензирование</strong>: Тщательно оцените последствия лицензирования, поскольку они напрямую влияют на краткосрочные и долгосрочные затраты. Модели с открытым исходным кодом (например, MIT, Apache 2.0 или аналогичные лицензии) обеспечивают гибкость при модификации и коммерческом использовании, предоставляя полный контроль над развертыванием, но требуя опыта работы с инфраструктурой. Собственные модели, доступные через API, обеспечивают удобство и простоту, но сопряжены с постоянными расходами и потенциальными проблемами конфиденциальности данных. Это решение особенно важно для приложений в регулируемых отраслях, где суверенитет данных или требования соответствия могут сделать самостоятельное размещение необходимым, несмотря на более высокие первоначальные инвестиции.</p></li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/model_research2_b0df75cb55.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="4-Evaluate-Candidate-Models" class="common-anchor-header">4. Оцените модели-кандидаты<button data-href="#4-Evaluate-Candidate-Models" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>После того как вы отобрали несколько моделей, настало время протестировать их на образцах данных. Вот ключевые факторы, которые следует учитывать:</p>
<ul>
<li><strong>Оценка:</strong> При оценке качества встраивания - особенно в приложениях для расширенного поиска (RAG) или поисковых системах - важно определить <em>, насколько точными, релевантными и полными</em> являются возвращаемые результаты. Ключевые метрики включают верность, релевантность ответа, точность контекста и отзыв. Такие фреймворки, как Ragas, DeepEval, Phoenix и TruLens-Eval, упрощают процесс оценки, предоставляя структурированные методики для оценки различных аспектов качества встраивания. Наборы данных не менее важны для полноценной оценки. Они могут быть созданы вручную для представления реальных случаев использования, синтетически сгенерированы LLM для проверки конкретных возможностей или созданы с помощью таких инструментов, как Ragas и FiddleCube, для нацеливания на определенные аспекты тестирования. Правильное сочетание набора данных и фреймворка зависит от конкретного приложения и уровня детализации оценки, который необходим для принятия уверенных решений.</li>
<li><strong>Бенчмарк производительности:</strong> Оцените модели на эталонах для конкретных задач (например, MTEB для поиска). Помните, что рейтинги существенно различаются в зависимости от сценария (поиск или классификация), наборов данных (общие или специфические для конкретной области, например BioASQ) и метрик (точность, скорость). Хотя эталонные показатели дают ценную информацию, они не всегда идеально подходят для реальных приложений. Проверьте лучшие результаты, которые соответствуют вашему типу данных и целям, но всегда проверяйте их с помощью собственных тестовых примеров, чтобы выявить модели, которые могут превышать эталонные показатели, но не работать в реальных условиях с вашими конкретными шаблонами данных.</li>
<li><strong>Нагрузочное тестирование:</strong> Для моделей, размещаемых на собственном хостинге, смоделируйте реалистичные производственные нагрузки, чтобы оценить производительность в реальных условиях. Измерьте пропускную способность, а также загрузку GPU и потребление памяти во время вычислений, чтобы выявить потенциальные узкие места. Модель, которая хорошо работает в изоляции, может стать проблемной при обработке одновременных запросов или сложных входных данных. Если модель слишком ресурсоемка, она может оказаться непригодной для крупномасштабных приложений или приложений реального времени, независимо от ее точности по эталонным метрикам.</li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/evaluate_candidate_models_3a7edd9cd7.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="5-Model-Integration" class="common-anchor-header">5. Интеграция модели<button data-href="#5-Model-Integration" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>После выбора модели настало время спланировать подход к интеграции.</p>
<ul>
<li><strong>Выбор весов:</strong> Решите, использовать ли заранее обученные веса для быстрого развертывания или провести тонкую настройку на данных, специфичных для конкретной области, для повышения производительности. Помните, что тонкая настройка может повысить производительность, но требует больших ресурсов. Подумайте, оправдывает ли прирост производительности дополнительную сложность.</li>
<li><strong>Самостоятельный хостинг против стороннего сервиса:</strong> Выбирайте подход к развертыванию, исходя из возможностей и требований вашей инфраструктуры. Самостоятельное размещение дает вам полный контроль над моделью и потоком данных, потенциально снижая стоимость каждого запроса при масштабировании и обеспечивая конфиденциальность данных. Однако для этого требуется опыт работы с инфраструктурой и постоянное обслуживание. Сторонние сервисы обработки выводов обеспечивают быстрое развертывание с минимальной настройкой, но вносят сетевые задержки, потенциальные ограничения на использование и постоянные расходы, которые могут стать значительными при масштабировании.</li>
<li><strong>Интеграционный дизайн:</strong> Продумайте дизайн API, стратегии кэширования, подход к пакетной обработке и выбор <a href="https://milvus.io/blog/what-is-a-vector-database.md">векторной базы данных</a> для эффективного хранения и запроса вкраплений.</li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/model_integration_8c8f0410c7.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="6-End-to-End-Testing" class="common-anchor-header">6. Сквозное тестирование<button data-href="#6-End-to-End-Testing" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>Перед полным развертыванием проведите сквозное тестирование, чтобы убедиться, что модель работает так, как ожидается:</p>
<ul>
<li><strong>Производительность</strong>: Всегда оценивайте модель на собственном наборе данных, чтобы убедиться, что она хорошо работает в вашем конкретном случае. Учитывайте такие метрики, как MRR, MAP и NDCG для качества поиска, точность, отзыв и F1 для точности, а также перцентили пропускной способности и задержки для операционной производительности.</li>
<li><strong>Надежность</strong>: Протестируйте модель в различных условиях, включая нестандартные ситуации и разнообразные входные данные, чтобы убедиться, что она работает стабильно и точно.</li>
</ul>
<p>
  <span class="img-wrapper">
    <img translate="no" src="https://assets.zilliz.com/end_to_end_testing_7ae244a73b.png" alt="" class="doc-image" id="" />
    <span></span>
  </span>
</p>
<h2 id="Summary" class="common-anchor-header">Резюме<button data-href="#Summary" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>Как мы видели в этом руководстве, выбор правильной модели встраивания требует соблюдения этих шести критических шагов:</p>
<ol>
<li>Определите бизнес-требования и тип задачи.</li>
<li>Проанализируйте характеристики данных и специфику области</li>
<li>Изучите доступные модели и условия их лицензирования</li>
<li>Тщательно оцените кандидатов по соответствующим эталонам и тестовым наборам данных</li>
<li>Планируйте подход к интеграции с учетом вариантов развертывания</li>
<li>Проведите комплексное сквозное тестирование перед внедрением в производство.</li>
</ol>
<p>Следуя этой схеме, вы сможете принять обоснованное решение, которое позволит сбалансировать производительность, стоимость и технические ограничения для вашего конкретного случая использования. Помните, что "лучшая" модель - это не обязательно модель с самыми высокими показателями - это модель, которая наилучшим образом отвечает вашим конкретным требованиям в рамках операционных ограничений.</p>
<p>Поскольку модели встраивания быстро развиваются, стоит периодически пересматривать свой выбор, поскольку появляются новые варианты, которые могут предложить значительные улучшения для вашего приложения.</p>
