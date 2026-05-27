# Mentor-Mentee Matching System

## 0. ОБЗОР ПРОЕКТА
Mentor-Mentee Matching System - рекомендательная система которая автоматически подбирает топ-5 менторов для каждого менти. Алгоритм оценивает каждую пару по четырём факторам: схожесть навыков, близость IT-областей, стаж и рейтинг ментора. Пользователь настраивает приоритеты через слайдеры (1–5), система рассчитывает индивидуальные веса.

Структура пайплайна:
1. Тетрадка - 1_ontology.ipynb, входные файлы - mentees.csv, mentors.csv, выходные данные - ontology_domains.csv, ontology_role_domain_mapping.csv, ontology_domain_similarity.csv
2. Тетрадка - 2_preprocessing.ipynb, входные данные - +ontology_*.csv, выходные данные - mentees_processed.csv, mentors_processed.csv, skill_vocabulary.csv
3. Тетрадка - 3_scoring.ipynb, входные данные - +*_processed.csv, mentor_ratings.csv, выходные данные - normalization_bounds.csv, mentee_weights.csv, recommendations.csv
4. Тетрадка - 4_evaluation.ipynb, входные данные - +все предыдущие, выходные данные - evaluation_results.csv
5. Тетрадка - 5_explainability.ipynb, входные данные - +все предыдущие, выходные данные - recommendations_explained.csv
6. Тетрадка - 6_boost_analytics.ipynb, входные данные - +все предыдущие, выходные данные - boost_analytics.csv
