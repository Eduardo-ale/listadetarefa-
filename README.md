

<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Tarefas</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: url('fundo.png') no-repeat center center fixed;
            background-size: cover;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background: rgba(255, 255, 255, 0.9);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }
        h2 { text-align: center; }
        input, button, select {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        ul {
            list-style: none;
            padding: 0;
        }
        li {
            background: #f9f9f9;
            padding: 10px;
            margin-top: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-radius: 5px;
        }
        .completed { text-decoration: line-through; color: gray; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Lista de Tarefas</h2>
        <input type="text" id="taskInput" placeholder="Nova tarefa">
        <button onclick="addTask()">Adicionar</button>
        <select id="filter" onchange="filterTasks()">
            <option value="all">Todas</option>
            <option value="pending">Pendentes</option>
            <option value="completed">Concluídas</option>
        </select>
        <ul id="taskList"></ul>
    </div>
    <script>
        let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
        
        function saveTasks() {
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }
        
        function renderTasks() {
            const taskList = document.getElementById("taskList");
            taskList.innerHTML = "";
            const filter = document.getElementById("filter").value;
            
            tasks.forEach((task, index) => {
                if (filter === "completed" && !task.completed) return;
                if (filter === "pending" && task.completed) return;
                
                const li = document.createElement("li");
                li.className = task.completed ? "completed" : "";
                li.innerHTML = `
                    <span onclick="toggleTask(${index})">${task.text}</span>
                    <button onclick="completeTask(${index})">✅</button>
                    <button onclick="editTask(${index})">✏️</button>
                    <button onclick="deleteTask(${index})">❌</button>
                `;
                taskList.appendChild(li);
            });
        }
        
        function addTask() {
            const taskInput = document.getElementById("taskInput");
            if (!taskInput.value.trim()) return;
            tasks.push({ text: taskInput.value, completed: false, completedAt: null });
            taskInput.value = "";
            saveTasks();
            renderTasks();
        }
        
        function completeTask(index) {
            tasks[index].completed = true;
            tasks[index].completedAt = new Date().toLocaleString();
            saveTasks();
            renderTasks();
        }
        
        function deleteTask(index) {
            tasks.splice(index, 1);
            saveTasks();
            renderTasks();
        }
        
        function editTask(index) {
            const newText = prompt("Edite a tarefa:", tasks[index].text);
            if (newText !== null) {
                tasks[index].text = newText;
                saveTasks();
                renderTasks();
            }
        }
        
        function filterTasks() {
            renderTasks();
        }
        
        renderTasks();
    </script>
</body>
</html>
