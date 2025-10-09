---
id: 36847e80-a693-4654-8365-76fb1a2daf2f
---

# ᐈ Как считать Retention rate, N-day Retention и Rolling Retention - GoPractice
#Omnivore

[Read on Omnivore](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c)
[Read Original](https://gopractice.ru/product/nday-retention-rollling-retention/)

## Highlights

> Классический Day N Retention (далее — Day N Retention) показывает, какой процент новых пользователей вернулись в продукт в конкретный день с момента прихода.
> 
> Rolling Retention показывает, какой процент новых пользователей вернулись в продукт в конкретный день с момента прихода или любой день после него.
> 
> Если Retention 2-го дня приложения равен 50%, это значит, что 50% новых пользователей этого приложения возвращаются в него **на 2-й день**.
> 
> Если Rolling Retention 2-го дня приложения равен 50%, это значит, что 50% новых пользователей приложения вернутся в него **на 2-й день или позже**. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#b32b53f3-72f3-43a5-935b-b12086c2ae85)  

> Для подавляющего большинства задач и продуктов N-day (week/month) Retention подходит лучше, чем Rolling Retention.
> 
> Но бывают редкие случаи, когда Rolling Retention удобнее. Обычно его применяют для продуктов, которые подразумевают достаточно редкое использование.
> 
> Например, если вы хотите проанализировать долгосрочную возвращаемость в приложение для покупки авиабилетов или бронирования отелей, то с этим поможет Rolling Retention. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#3a6e00fd-09a9-4cbc-8051-4dc420efbf60)  

> Rolling Retention позволит ответить на вопрос: «А какая часть пользователей хотя бы раз вернулась в наше приложение после 30 дня?». Но и здесь можно обойтись без Rolling Retention, а следить за обычным месячным Retention, либо же создать квартальный или годичный Retention (за годичным Retention, например, следят в Airbnb). [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#098af18a-8880-4304-ae47-de3cc2f3dcd1)  

> При расчете на основе 24-часовых окон для конкретного пользователя Retention будет считаться на основе индивидуальных временных интервалов. День 0 конкретного пользователя начинается с момента первого запуска продукта и заканчивается через 24 часа. День 1 начинается спустя 24 часа после первого запуска и заканчивается спустя 48 часов. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#df1cae68-b5ca-43e8-a688-f9c0d32d5dc7)  

> Пусть Retention 1 дня при расчете на основе 24-часовых окон равен 10% для когорты из 1000 новых пользователей, которые пришли 1 октября. Это значит, что 100 из этих пользователей зашли в приложение между 24 и 48 часом после своего первого запуска. Важно понимать, что временной интервал 1 дня для разных пользователей в данном случае будет индивидуальным. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#1fa5c04e-c546-4975-826d-135a6d429060)  

> Amplitude, аналитическая система, которую вы используете, по умолчанию считает Retention на основе 24-часовых окон. При этом Appsflyer по умолчанию считает Retention на основе календарных дней. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#2b7c7d80-0b72-4dff-9b82-c11069047e30)  

> Расчет метрики Retention rate на основе 24-часовых окон дает более честный ответ на вопрос о том, как ваш продукт возвращает пользователей. Но такой метод расчета Retention требует больше времени. Вы получите финальное значение метрики для пользователей, которые запустили продукт в конкретный день, лишь спустя 2 календарных дня (48 часов с момента прихода последнего пользователя). При расчете же Retention на основе календарных дней, значение Retention 1 дня будет доступно уже через 24 часа. [⤴️](https://omnivore.app/me/https-gopractice-ru-product-nday-retention-rollling-retention-18fe6e6d37c#da5f9f72-8adc-4a3a-9475-4f466f85503a)  

