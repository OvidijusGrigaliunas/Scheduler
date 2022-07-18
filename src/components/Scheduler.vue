<script setup>
import SchedulerItem from './SchedulerItem.vue'
import TaskEditor from './TaskEditor.vue'
import WeekSelector from './WeekSelector.vue';
import PersonSelector from './PersonSelector.vue';

</script>

<script>
// Čia yra parent, visi kiti jo child
export default {
  data() {
    return {
      taskArray: [
        // Jei rašo nuo 10:00 iki 15:00, tai reiškia , kad nuo 15:00 jau laisvas
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/18", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/18", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/18", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/18", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/18", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/18", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/18", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/18", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/18", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/18", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13, taskHourEnd: 13.25, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13, taskHourEnd: 13.5, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Vardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Vardenis', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Vardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Vardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Vardenis', taskImportance: 3 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/18", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/18", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/18", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/18", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/18", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/18", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/18", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/18", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/18", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/18", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 11.75, taskHourEnd: 12, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13, taskHourEnd: 13.25, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13, taskHourEnd: 13.5, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Bonilla', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Bonilla', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Bonilla', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Bonilla', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Bonilla', taskImportance: 3 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/19", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/19", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/19", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/19", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/19", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/20", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/20", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/20", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/20", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/20", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/21", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/21", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/21", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/21", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/21", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task5', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 11.5, taskHourEnd: 11.75, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task6', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 14, taskHourEnd: 15, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task7', taskDesc: 'task1 desc', taskDay: "2022/07/22", taskHourStart: 15, taskHourEnd: 16, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task8', taskDesc: 'task2 desc', taskDay: "2022/07/22", taskHourStart: 16, taskHourEnd: 16.25, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task9', taskDesc: 'task3 desc', taskDay: "2022/07/22", taskHourStart: 16.25, taskHourEnd: 16.5, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task10', taskDesc: 'task4 desc', taskDay: "2022/07/22", taskHourStart: 16.5, taskHourEnd: 16.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task11', taskDesc: 'task5 desc', taskDay: "2022/07/22", taskHourStart: 16.75, taskHourEnd: 17, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.5, taskHourEnd: 10.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/18", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/18", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13, taskHourEnd: 13.25, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13, taskHourEnd: 13.75, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13, taskHourEnd: 13.5, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Pavardenis', taskImportance: 5 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Pavardenis', taskImportance: 3 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 13.75, taskHourEnd: 14, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/18", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/19", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Pavardenis', taskImportance: 4 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/20", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Pavardenis', taskImportance: 2 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/21", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Pavardenis', taskImportance: 1 },
        { taskName: 'task12', taskDesc: 'task6 desc', taskDay: "2022/07/22", taskHourStart: 10.75, taskHourEnd: 11, taskTarget: 'Pavardenis', taskImportance: 3 },
      ],
      people: [
        { name: "Vardenis", shiftStart: 8, shiftEnd: 17, breakStart: 12, breakEnd: 13, workDays: [true, true, true, true, true, false, false] },
        { name: "Pavardenis", shiftStart: 9, shiftEnd: 19, breakStart: 12, breakEnd: 13, workDays: [true, true, true, true, true, false, false] },
        { name: "Bonilla", shiftStart: 10, shiftEnd: 19, breakStart: 11, breakEnd: 13, workDays: [true, true, true, true, true, false, true] },
        { name: "Hobbs", shiftStart: 7, shiftEnd: 16, breakStart: 10, breakEnd: 11, workDays: [true, false, true, true, true, false, false] }
      ],
      selectedHour: new Date().getHours(),
      selectedDate: new Date(),
      selectedDay: new Date().getDay(),
      selectedPersonIndex: 0,
      windowHeight: window.screen.availHeight - 197,
      filteredTasksByUsers: [],
      filteredTasks: [],
      currentWeek: [],
      itemTaskInfo: [],
      taskEditorKey: 30,
      // Veikia su 0.25, 0.5 ir 1
      // Jei 0.25, laiko tarpai yra 15min
      // 0.5 - 30 min; 1 - 1h
      // su vertėmis didesnėmis negu 1 neveikia
      // Pagal viską turėtų veikti su 0.1 ir 0.2 (6min ir 12min), bet vertės gaunamos su daug skaičių po kablelio.
      // Turi veikti su 1/2^x vertėmis, tik reikėtų pataisyti laiko formativimą truputi
      // Jei timeScale yra 0 susidaro infinite loop
      // Turi didelę įtaką ant performance
      timeScale: 0.25
    }
  },
  computed: {
    getShiftTimeArray() {
      let result = [];
      this.people.forEach((person, index) => {
        result[index] = [];
        for (let i = person.shiftStart; i < person.shiftEnd; i = i + this.timeScale) {
          result[index].push(i)
        }
      });
      return result;
    },
    getBreakTimeArray() {
      let result = [];
      this.people.forEach((person, index) => {
        result[index] = [];
        for (let i = person.breakStart; i < person.breakEnd; i = i + this.timeScale) {
          result[index].push(i);
        }
      });
      return result;
    },
    selectedDateFormatting() {
      let result = this.selectedDate.getFullYear() + "/"
      let month = this.selectedDate.getMonth() + 1;
      // Su if nustatome ar reikia pridėti 0 pradžioje,
      // kad gautume yyyy/mm/dd datos formatą
      if (month < 10) {
        result = result + 0 + month + "/";
      } else {
        result = result + month + "/";
      }
      if (this.selectedDate.getDate() < 10) {
        result = result + 0 + this.selectedDate.getDate();
      } else {
        result = result + this.selectedDate.getDate();
      }
      return result;
    },
    getResolutionHeight() {
      let height = this.windowHeight;
      if (height < 750 && height > 1000) {
        height = height + 85;
      }
      return height;
    },
    setHeight() {
      let height = this.getResolutionHeight + 'px';
      return { height: height }
    },
    formatTime() {
      let timeArray = [];
      let text;
      for (let i = 0; i < 10; i = i + this.timeScale) {
        text = ('0' + (i - (i % 1)) + ':' + (60 * (i % 1)));
        if (text.length != 5) {
          text = text.substring(0, 4) + 0 + text.substring(4);
        }
        timeArray.push(text)
      }
      for (let i = 10; i < 24; i = i + this.timeScale) {
        text = ((i - (i % 1)) + ':' + (60 * (i % 1)));
        if (text.length != 5) {
          text = text.substring(0, 4) + 0 + text.substring(4);
        }
        timeArray.push(text)
      }
      return timeArray;
    }
  },
  mounted() {
    window.onresize = () => {
      this.windowHeight = window.screen.availHeight - 197;
    }
  },
  created() {
    this.taskInfoArray = new Array(7).fill().map(() => new Array(this.getShiftTimeArray[0].length).fill({ name: null, importance: null }));
    this.createWeek(this.selectedDate)
    this.filterTaskByUsers();
  },
  methods: {
    // Keičiant vartotoją dažnai pasirinkta valanda išeidavo iš darbo laiko ribų
    // Ši funkcija tai pataiso
    getSelectedHour() {
      if (
        !this.getShiftTimeArray[this.selectedPersonIndex].includes(this.selectedHour) ||
        this.getBreakTimeArray[this.selectedPersonIndex].includes(this.selectedHour)
      ) {
        this.selectedHour = this.people[this.selectedPersonIndex].shiftStart;
        this.taskEditorKey = this.taskEditorKey * (-1);
      }
    },
    createNewTask(name, desc, startsAt, importance, endsAt) {
      let object = {
        taskName: name,
        taskDesc: desc,
        taskDay: this.selectedDateFormatting,
        taskHourStart: startsAt,
        taskHourEnd: endsAt,
        taskTarget: this.people[this.selectedPersonIndex].name,
        taskImportance: importance
      }
      this.selectedHour = startsAt;
      this.taskArray.push(object);
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    deleteTask(taskToDelete) {
      for (let i = 0; i < this.taskArray.length; i++) {
        if (taskToDelete === this.taskArray[i]) {
          this.taskArray.splice(i, 1);
          break;
        }
      }
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    editTask(taskToChange, name, desc, startsAt, importance, endsAt) {
      let taskIndex = this.taskArray.indexOf(taskToChange);
      this.taskArray[taskIndex] = {
        taskName: name,
        taskDesc: desc,
        taskDay: this.selectedDateFormatting,
        taskHourStart: startsAt,
        taskHourEnd: endsAt,
        taskTarget: this.people[this.selectedPersonIndex].name,
        taskImportance: importance
      };
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    timeSelection(dateIndex, hour) {
      this.selectedDay = dateIndex + 1;
      this.selectedHour = hour;
      this.selectedDate = this.currentWeek[dateIndex];
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    addDays(date, days) {
      let newDate = new Date(date);
      newDate.setDate(newDate.getDate() + days);
      return newDate;
    },
    // Sudarome savaitę pagal pasirinktą diena
    createWeek(date) {
      let currentWeekDay = date.getDay();
      // kai yra sekmadienis, vertė būna 0
      // tokiu atveju 0 reikia pasiversti į 7
      if (currentWeekDay === 0) {
        currentWeekDay = 7;
      }
      for (let i = 0; i < 7; i++) {
        // "i + 1 - currentWeekDay" reikalingas, kad savaitės dienos eitų iš eilės
        // (Monday, tuesday ir t.t.)
        this.currentWeek[i] = this.addDays(date, i + 1 - currentWeekDay);
      }
      this.getSelectedHour();
    },
    changeWeek(direction) {
      this.selectedDate = this.addDays(this.selectedDate, direction);
      this.createWeek(this.selectedDate);
      this.filterTasksByWeek(this.currentWeek);
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    changePerson(direction) {
      this.selectedPersonIndex = this.selectedPersonIndex + direction
      // Jei išeiname iš masyvo ribų, pagal direction nustatome ar turime peršokti į masyvo
      // pradžią ar pabaigą. Jei direction -1 - pabaiga, jei 1 - pradžia;
      if (!this.people[this.selectedPersonIndex]) {
        this.selectedPersonIndex = this.people.length - (this.selectedPersonIndex * direction);
      }
      this.getSelectedHour();
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    filterTasksByWeek() {
      let formattedWeek = this.dateFormatting(this.currentWeek);
      // Naudojame 2d array, kad galėtume lengvai paskirstyti užduotis į
      // dienas.
      for (let i = 0; i < 7; i++) {
        this.filteredTasks[i] = []
      }
      this.filteredTasksByUsers.forEach(task => {
        for (let i = 0; i < 7; i++) {
          if (task.taskDay === formattedWeek[i]) {
            this.filteredTasks[i].push(task);
            break;
          }
        }
      });
      this.generateTaskInfoForItems();
    },
    filterTaskByUsers() {
      this.filteredTasksByUsers = this.taskArray.filter(task => task.taskTarget === this.people[this.selectedPersonIndex].name);
      this.filterTasksByWeek();
    },
    dateFormatting(week) {
      let result = [];
      let formattedDay = '';
      let month;
      week.forEach(day => {
        formattedDay = day.getFullYear() + "/";
        month = day.getMonth() + 1;
        // Su if nustatome ar reikia pridėti 0 pradžioje,
        // kad gautume yyyy/mm/dd datos formatą
        if (month < 10) {
          formattedDay = formattedDay + 0 + month + "/";
        } else {
          formattedDay = formattedDay + month + "/";
        }
        if (day.getDate() < 10) {
          formattedDay = formattedDay + 0 + day.getDate()
        } else {
          formattedDay = formattedDay + day.getDate();
        }
        result.push(formattedDay)
      });
      return result;
    },
    // Sugeneruoja 2d array, pagal kurią galime žinoti ar langelis turi užduotį.
    generateTaskInfoForItems() {
      let taskInfoArray = new Array(7).fill().map(() => new Array(this.getShiftTimeArray[this.selectedPersonIndex].length).fill({ name: null, importance: null }));
      for (let i = 0; i < 7; i++) {
        this.filteredTasks[i].forEach(task => {
          taskInfoArray[i] = this.updateColumnTaskInfo(taskInfoArray[i], task)
        })
      }
      this.itemTaskInfo = taskInfoArray;
    },
    updateColumnTaskInfo(taskArray, task) {
      let taskHourEndIndex = (task.taskHourEnd - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
      let taskHourStartIndex = (task.taskHourStart - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
      for (let i = taskHourStartIndex; i < taskHourEndIndex; i++) {
        taskArray[i] = { name: task.taskName, importance: task.taskImportance }
      }
      return taskArray;
    }
  }
}
</script>

<template>
  <PersonSelector :peopleList='people' :selectedIndex='selectedPersonIndex' @personChange="changePerson" />
  <div class="container">
    <div class="gridContainer">
      <div class="gridAndDateContainer">
        <div class="selectContainer">
          <WeekSelector :weekRange='currentWeek' @weekChange="changeWeek" />
        </div>
        <div class="grid" :style="setHeight">
          <div class="time blankRectangle"></div>
          <div class="weekDay">
            <p>Monday</p>
          </div>
          <div class="weekDay">
            <p>Tuesday</p>
          </div>
          <div class="weekDay">
            <p>Wednesday</p>
          </div>
          <div class="weekDay">
            <p>Thursday</p>
          </div>
          <div class="weekDay">
            <p>Friday</p>
          </div>
          <div class="weekDay">
            <p>Saturday</p>
          </div>
          <div class="weekDay">
            <p>Sunday</p>
          </div>
          <template v-for='(i, hourIndex) in getShiftTimeArray[selectedPersonIndex]'>
            <div class="time">
              <h1>{{ formatTime[i / timeScale] }}</h1>
            </div>
            <template v-for="(day, dayIndex) in currentWeek">
              <SchedulerItem
                v-if="!people[selectedPersonIndex].workDays[dayIndex] || getBreakTimeArray[selectedPersonIndex].includes(i)"
                :noWork='true' />
              <SchedulerItem v-else :dateIndex='dayIndex' :hour='i' :taskName='itemTaskInfo[dayIndex][hourIndex].name'
                :taskImportance='itemTaskInfo[dayIndex][hourIndex].importance' @timeSelected="timeSelection" />
            </template>
          </template>
        </div>
      </div>
      <TaskEditor :key="taskEditorKey" :resolution='getResolutionHeight' :date='selectedDateFormatting'
        :tasks='filteredTasks[selectedDay - 1]' :hour='selectedHour' :formattedTime='formatTime'
        :breakTime='getBreakTimeArray[selectedPersonIndex]' :shiftTime='getShiftTimeArray[selectedPersonIndex]'
        :timeScale='timeScale' @NewTask="createNewTask" @taskDeletion="deleteTask" @taskEdit='editTask' />
    </div>
  </div>
</template>

<style scoped>
.gridContainer {
  display: flex;
  -webkit-touch-callout: none;
  /* iOS Safari */
  -webkit-user-select: none;
  /* Safari */
  -khtml-user-select: none;
  /* Konqueror HTML */
  -moz-user-select: none;
  /* Old versions of Firefox */
  -ms-user-select: none;
  /* Internet Explorer/Edge */
  user-select: none;
  /* Non-prefixed version, currently */

}

.gridAndDateContainer {
  width: 70%;
  -ms-overflow-style: none;
  /* IE and Edge */
  scrollbar-width: none;
  /* Firefox */
}

.container {
  margin: auto;
  overflow-x: hidden;
  width: 100%;
  border: solid #555B6E;
  border-width: 1px 1px 1px 1px;
}

.selectContainer {
  display: flex;
  align-content: center;
  width: 100%;
  -webkit-touch-callout: none;
  /* iOS Safari */
  -webkit-user-select: none;
  /* Safari */
  -khtml-user-select: none;
  /* Konqueror HTML */
  -moz-user-select: none;
  /* Old versions of Firefox */
  -ms-user-select: none;
  /* Internet Explorer/Edge */
  user-select: none;
  /* Non-prefixed version, currently*/
}

.grid {
  display: grid;
  width: 100%;
  row-gap: 0;
  grid-template-columns: 9.1% repeat(7, 13%);
  overflow-y: scroll;
  -ms-overflow-style: none;
  /* IE and Edge */
  scrollbar-width: none;
}

.grid::-webkit-scrollbar {
  display: none;
}

.time {
  width: 100%;
  height: 50px;
  border: solid #555B6E;
  border-width: 1px;
  text-align: center;
  font-weight: bold;
  background-color: #78a1bb;
  overflow: hidden;
  color: white;
}

.blankRectangle {
  height: 30px;
}

.time h1 {
  line-height: 40px;
  font-size: 20px;
}

.weekDay {
  width: 100%;
  height: 30px;
  border: solid #555B6E;
  border-width: 1px;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  background-color: #78a1bb;
  overflow: hidden;
  color: white;
}

.selected {
  background-color: #277DA1;
}

@media (max-width: 800px) {
  .time h1 {
    line-height: 36px;
    font-size: 16px;
  }
}

@media (max-width: 600px) {
  .time h1 {
    line-height: 30px;
    font-size: 10px;
  }
}
</style>