html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Student Directory</title>
    <link rel="stylesheet" href="src/css/style.css" />
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
</head>
<body>
    <header>
        <h1><i class="fas fa-graduation-cap"></i> Student Directory</h1>
        <input type="text" id="search" placeholder="Search students..." />
    </header>

    <main>
        <div id="student-list" class="grid"></div>
    </main>

    <script src="main.js"></script>
</body>
</html>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    color: #333;
}

header {
    background-color: #333;
    color: white;
    padding: 20px;
    text-align: center;
}

header h1 {
    margin-bottom: 15px;
}

#search {
    width: 90%;
    max-width: 500px;
    padding: 10px;
    border: none;
    border-radius: 4px;
    font-size: 16px;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
    padding: 20px;
    max-width: 1200px;
    margin: 0 auto;
}

.student-card {
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    overflow: hidden;
    transition: transform 0.3s;
}

.student-card:hover {
    transform: translateY(-4px);
}

.card-image {
    width: 100%;
    height: 180px;
    object-fit: cover;
}

.card-content {
    padding: 15px;
}

.card-content h3 {
    margin-bottom: 8px;
    color: #222;
}

.card-desc {
    font-size: 14px;
    color: #666;
    margin-bottom: 12px;
}

.social-links {
    display: flex;
    gap: 10px;
    margin-bottom: 12px;
}

.social-links a {
    color: #555;
    font-size: 18px;
    text-decoration: none;
}

.social-links a:hover {
    color: #007bff;
}

.btn {
    display: inline-block;
    padding: 8px 16px;
    background-color: #007bff;
    color: white;
    text-decoration: none;
    border-radius: 4px;
    font-size: 14px;
}

.btn:hover {
    background-color: #0056b3;
}

/* Profile Page */
.profile-container {
    max-width: 700px;
    margin: 30px auto;
    padding: 0 20px;
}

.profile-card {
    background: white;
    border-radius: 8px;
    box-shadow: 0 3px 10px rgba(0,0,0,0.1);
    overflow: hidden;
}

.profile-header {
    text-align: center;
    padding: 25px;
    background-color: #f8f9fa;
}

.profile-img {
    width: 160px;
    height: 160px;
    border-radius: 50%;
    object-fit: cover;
    border: 4px solid white;
    margin-bottom: 15px;
}

.profile-body {
    padding: 25px;
}

.profile-body h2 {
    color: #333;
    margin: 20px 0 10px;
    border-bottom: 2px solid #007bff;
    padding-bottom: 5px;
}

.skills-list {
    list-style: none;
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin: 10px 0;
}

.skills-list li {
    background: #e9ecef;
    padding: 6px 12px;
    border-radius: 20px;
    font-size: 14px;
}

.back-btn {
    margin: 20px;
}
// Factory Pattern
class StudentFactory {
    static createStudent(data) {
        return {
            id: data.id,
            name: data.name,
            shortDesc: data.shortDesc,
            image: data.image,
            profileUrl: data.profileUrl,
            social: data.social
        };
    }
}

// Student Data — Mark Joshua
const studentsData = [
    {
        id: "mark joshua",
        name: "Mark Joshua",
        shortDesc: "Computer science student focused on web development, clean code, and building useful applications.",
        image: "https://picsum.photos/id/22/300/200",
        profileUrl: "students/mark-joshua.html",
        social: {
            facebook: "https://facebook.com",
            github: "https://github.com",
            linkedin: "https://linkedin.com"
        }
    }
];

// Create student instances
const students = studentsData.map(data => StudentFactory.createStudent(data));

// Render Cards
function renderStudents(list) {
    const container = document.getElementById("student-list");
    container.innerHTML = "";

    list.forEach(student => {
        const card = document.createElement("div");
        card.className = "student-card";
        card.innerHTML = `
            <img src="${student.image}" alt="${student.name}" class="card-image">
            <div class="card-content">
                <h3>${student.name}</h3>
                <p class="card-desc">${student.shortDesc}</p>
                <div class="social-links">
                    <a href="${student.social.facebook}" target="_blank"><i class="fab fa-facebook"></i></a>
                    <a href="${student.social.github}" target="_blank"><i class="fab fa-github"></i></a>
                    <a href="${student.social.linkedin}" target="_blank"><i class="fab fa-linkedin"></i></a>
                </div>
                <a href="${student.profileUrl}" class="btn">View Profile</a>
            </div>
        `;
        container.appendChild(card);
    });
}

// Observer Pattern — Search
function setupSearch() {
    const searchInput = document.getElementById("search");
    searchInput.addEventListener("input", e => {
        const keyword = e.target.value.toLowerCase().trim();
        const filtered = students.filter(student =>
            student.name.toLowerCase().includes(keyword) ||
            student.shortDesc.toLowerCase().includes(keyword)
        );
        renderStudents(filtered);
    });
}

// In