# Mentor-Mentee Matching System

## 0. ОБЗОР ПРОЕКТА
Mentor-Mentee Matching System - рекомендательная система которая автоматически подбирает топ-5 менторов для каждого менти. Алгоритм оценивает каждую пару по четырём факторам: схожесть навыков, близость IT-областей, стаж и рейтинг ментора. Пользователь настраивает приоритеты через слайдеры (1–5), система рассчитывает индивидуальные веса.

Структура пайплайна:
| Тетрадка | Входные файлы | Выходные файлы |
| :--- | :---: | ---: |
| 1_ontology.ipynb | mentees.csv, mentors.csv | ontology_domains.csv, ontology_role_domain_mapping.csv, ontology_domain_similarity.csv | 
| 2_preprocessing.ipynb | +ontology_*.csv | mentees_processed.csv, mentors_processed.csv, skill_vocabulary.csv |
| 3_scoring.ipynb | + *_processed.csv, mentor_ratings.csv | normalization_bounds.csv, mentee_weights.csv, recommendations.csv |
| 4_evaluation.ipynb | + все предыдущие | evaluation_results.csv |
| 5_explainability.ipynb | + все предыдущие | recommendations_explained.csv |
| 6_boost_analytics.ipynb | + все предыдущие | boost_analytics.csv |

## 1. ДАННЫЕ И ИХ ОСОБЕННОСТИ
### 1.1 Состав датасета
| Файл | Строк | Сожержание | 
| :--- | :---: | ---: |
| mentees.csv | 5000 | профили менти: level, domain, profession, skills, goal, language, format |
| mentors.csv | 2000 | профили менторов: + experience_years, available, boosted, boost_k |
| mentors_ratings.csv | 2000 | рейтинги: N(3.8, 0.7) в диапазоне [1, 5] |

### 1.2 Пропуски в данных
~35% менти и ~32% менторов имеют хотя бы один пустой атрибут. Это реалистичная ситуация для любой платформы - пользователи не заполняют профили полностью.

*Почему не удаляем профили с пропусками? Потому что это исключило бы треть пользователей. Вместо этого применяем честное наказание: система даёт рекомендации, но score ниже — стимул заполнить профиль.*

### 1.3 Структура данных о менторах 

| Поле | Описание | Использование |
| :--- | :---: | ---: |
| level | junior/middle/senior/lead | Только в жесткой фильтрации - не в формуле score |
| skills | Навыки через ';' | Jaccard similarity с навыками менти |
| experience_years | Стаж в годах | Нормализуется: log(years+1)/log(21) |
| domain + profession | IT-область и должность | domain_vector через онтологию |
| available | Доступен ли для менторства | Жесткая фильтрация |
| boosted + boost_k | Платное продвижение | Буст финального score, если organic >= 0.3 |
