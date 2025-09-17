SELECT:
 1. Selezionare tutti gli studenti nati nel 1990 (160)
 - SELECT *
   FROM `students`
   WHERE `date_of_birth` > "1990-01-01"
   AND `date_of_birth` < "1990-12-31"
 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   SELECT *
   FROM `courses`
   WHERE `cfu` > "10"
 3. Selezionare tutti gli studenti che hanno più di 30 anni
   SELECT *
   FROM `students`
   - WHERE `date_of_birth` < "1995-12-31" = SBAGLIATO -
   WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30
 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
 laurea (286)
   SELECT *
   FROM `courses`
   WHERE `year` = "1"
   AND `period` = "I semestre"
 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
 20/06/2020 (21)
   SELECT *
   FROM `exams`
   WHERE `date` = "2020-06-20"
   AND HOUR (`hour`) > 14
 6. Selezionare tutti i corsi di laurea magistrale (38)
   SELECT *
   FROM `degrees`
   WHERE `level` LIKE "Magistrale"
 7. Da quanti dipartimenti è composta l'università? (12)
   SELECT COUNT(*) AS `num_departments`
   FROM `departments`
 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
   SELECT COUNT(*) AS `phone_teachers`
   FROM `teachers`
   WHERE `phone` IS NULL

 
 
 GROUP BY:
 1. Contare quanti iscritti ci sono stati ogni anno
   SELECT COUNT(*) AS `num_iscritti`, YEAR(`enrolment_date`) AS `year`
   FROM `students`
   GROUP BY `year`
 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
   SELECT COUNT(*) AS `num_teachers`, `office_address`
   FROM `teachers`
   GROUP BY `office_address`
 3. Calcolare la media dei voti di ogni appello d'esame
   SELECT COUNT(*) AS `media_voti`, `exam_id`
   FROM `exam_student`
   GROUP BY `exam_id`
 4. Contare quanti corsi di laurea ci sono per ogni dipartimenti
   SELECT COUNT(*) `num_degree`, `department_id`
   FROM `degrees`
   GROUP BY `department_id`



JOIN:
 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
  SELECT `degrees`.`name`, `students`.`name`, `students`.`id`, `degrees`.`id` AS `id degrees`
  FROM `degrees`
  JOIN `students` ON `degrees`.`id` = `students`.`degree_id`
  WHERE `degrees`.`name` = "Corso di Laurea in Economia"
 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
 Neuroscienze
  SELECT `departments`.`name` AS `department`, `degrees`.`name` AS `degree`
  FROM `departments`
  JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id`
  WHERE `departments`.`name` = "Dipartimento di Neuroscienze"
  AND `degrees`.`name` = "Corso di Laurea Magistrale"
 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
  SELECT `courses`.`id` AS `id course`, `teachers`.`id` AS `id teachers`, `teachers`.`name` AS `teachers`, `courses`.`name` AS `course`
  FROM `courses`
  JOIN `course_teacher` ON `course_teacher`.`course_id`= `courses`.`id`
  JOIN `teachers` ON `teachers`.`id`= `course_teacher`.`teacher_id`
  WHERE `teachers`.`id` = "44"
 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
 sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
 nome
  SELECT `students`.`name`, `students`.`surname`, `degrees`.`id` AS `id degree`, `degrees`.`name` AS `degree`, `departments`.`id` AS `id department`, `departments`.`name` AS `departments`
  FROM `degrees`
  JOIN `students` ON `degrees`.`id` = `students`.`degree_id`
  JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
  ORDER BY `students`.`name`, `students`.`surname` ASC
 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
 6. Selezionare tutti i docenti che insegnano nel Dipartimento di
 Matematica (54)
 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
 per ogni esame, stampando anche il voto massimo. Successivamente,
 filtrare i tentativi con voto minimo 18.