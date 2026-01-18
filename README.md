# NeedinAd-
Anonymous question and answer platform. Only the owner posts questions, everyone can reply anonymously 
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NeedinAd - Savol bitta. Fikrlar ko'p.</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
      background: #0f0f0f;
      color: #fff;
      padding: 24px;
      min-height: 100vh;
    }

    .container {
      max-width: 800px;
      margin: 0 auto;
    }

    h1 {
      font-size: 36px;
      margin-bottom: 10px;
    }

    .subtitle {
      color: #aaa;
      margin-bottom: 20px;
    }

    .btn {
      padding: 8px 16px;
      background: #333;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 14px;
      margin-bottom: 20px;
    }

    .btn:hover {
      background: #444;
    }

    .box {
      background: #1a1a1a;
      padding: 16px;
      border-radius: 8px;
      margin-bottom: 16px;
    }

    .box h3 {
      margin: 0 0 12px 0;
      font-size: 18px;
    }

    input, textarea {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      background: #222;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-family: Arial, sans-serif;
    }

    textarea {
      min-height: 80px;
      resize: vertical;
    }

    .btn-primary {
      padding: 10px;
      background: #fff;
      color: #000;
      border: none;
      width: 100%;
      cursor: pointer;
      border-radius: 4px;
      font-weight: bold;
    }

    .btn-primary:hover {
      background: #ddd;
    }

    .answer {
      background: #222;
      padding: 10px;
      border-radius: 6px;
      margin-bottom: 8px;
      color: #ddd;
      word-wrap: break-word;
    }

    .question-display {
      margin: 0;
      font-size: 16px;
      line-height: 1.5;
      background: #222;
      padding: 12px;
      border-radius: 6px;
    }

    .empty-state {
      color: #888;
      font-style: italic;
      margin: 0;
    }

    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.8);
      align-items: center;
      justify-content: center;
      z-index: 1000;
    }

    .modal.active {
      display: flex;
    }

    .modal-content {
      background: #1a1a1a;
      padding: 24px;
      border-radius: 8px;
      max-width: 400px;
      width: 90%;
    }

    .modal-content h3 {
      margin: 0 0 16px 0;
    }

    .modal-buttons {
      display: flex;
      gap: 10px;
    }

    .btn-success {
      flex: 1;
      padding: 10px;
      background: #4CAF50;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-weight: bold;
    }

    .btn-danger {
      flex: 1;
      padding: 10px;
      background: #f44336;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-weight: bold;
    }

    .archive-item {
      background: #222;
      padding: 16px;
      border-radius: 8px;
      margin-bottom: 16px;
    }

    .archive-header {
      display: flex;
      justify-content: space-between;
      align-items: start;
      margin-bottom: 12px;
    }

    .archive-question {
      margin: 0;
      font-size: 16px;
      color: #4CAF50;
      flex: 1;
    }

    .archive-date {
      font-size: 12px;
      color: #888;
      margin-left: 12px;
    }

    .archive-answers {
      padding-left: 12px;
      border-left: 3px solid #333;
    }

    .archive-count {
      margin: 0 0 8px 0;
      font-size: 14px;
      color: #aaa;
    }

    .archive-answer {
      background: #1a1a1a;
      padding: 8px;
      border-radius: 4px;
      margin-bottom: 6px;
      font-size: 14px;
      color: #ccc;
    }

    .archive-more {
      font-size: 12px;
      color: #666;
      font-style: italic;
      margin: 8px 0 0 0;
    }

    #mainView, #archiveView {
      display: none;
    }

    #mainView.active, #archiveView.active {
      display: block;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Header -->
    <div>
      <h1>NeedinAd</h1>
      <p class="subtitle">Savol bitta. Fikrlar ko'p.</p>
      <button class="btn" id="toggleViewBtn">📚 Arxivni ko'rish</button>
    </div>

    <!-- Main View -->
    <div id="mainView" class="active">
      <!-- Owner: Add Question -->
      <div class="box">
        <h3>Savol joylash (faqat egasi) 🔐</h3>
        <input type="text" id="questionInput" placeholder="Savolni yozing..." />
        <button class="btn-primary" id="addQuestionBtn">Savolni joylash</button>
      </div>

      <!-- Current Question -->
      <div class="box">
        <h3>📌 Hozirgi Savol</h3>
        <div id="currentQuestion">
          <p class="empty-state">Hozirda savol yo'q</p>
        </div>
      </div>

      <!-- Add Answer -->
      <div class="box">
        <h3>💬 Anonim javob qoldirish</h3>
        <textarea id="answerInput" placeholder="Fikringizni yozing..."></textarea>
        <button class="btn-primary" id="addAnswerBtn">Javob yuborish</button>
      </div>

      <!-- Answers -->
      <div class="box">
        <h3>💭 Javoblar <span id="answerCount">(0)</span></h3>
        <div id="answersList">
          <p class="empty-state">Hozircha javoblar yo'q. Birinchi bo'lib javob qoldiring!</p>
        </div>
      </div>
    </div>

    <!-- Archive View -->
    <div id="archiveView">
      <div class="box">
        <h2 style="margin: 0 0 20px 0; font-size: 24px;">📚 Arxiv</h2>
        <div id="archiveList">
          <p class="empty-state">Arxivda hech narsa yo'q</p>
        </div>
      </div>
    </div>

    <!-- Password Modal -->
    <div id="passwordModal" class="modal">
      <div class="modal-content">
        <h3>Parolni kiriting</h3>
        <input type="password" id="passwordInput" placeholder="Ega paroli..." />
        <div class="modal-buttons">
          <button class="btn-success" id="confirmPasswordBtn">Tasdiqlash</button>
          <button class="btn-danger" id="cancelPasswordBtn">Bekor qilish</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    const OWNER_PASSWORD = 'owner123';
    let currentView = 'main';

    // Get data from localStorage
    function getData() {
      const current = localStorage.getItem('needinad_current');
      const archive = localStorage.getItem('needinad_archive');
      return {
        current: current ? JSON.parse(current) : null,
        archive: archive ? JSON.parse(archive) : []
      };
    }

    // Save data to localStorage
    function saveData(current, archive) {
      if (current) {
        localStorage.setItem('needinad_current', JSON.stringify(current));
      }
      if (archive) {
        localStorage.setItem('needinad_archive', JSON.stringify(archive));
      }
    }

    // Escape HTML to prevent XSS
    function escapeHtml(text) {
      const div = document.createElement('div');
      div.textContent = text;
      return div.innerHTML;
    }

    // Toggle between main and archive view
    document.getElementById('toggleViewBtn').addEventListener('click', () => {
      if (currentView === 'main') {
        currentView = 'archive';
        document.getElementById('mainView').classList.remove('active');
        document.getElementById('archiveView').classList.add('active');
        document.getElementById('toggleViewBtn').textContent = '← Orqaga';
        renderArchive();
      } else {
        currentView = 'main';
        document.getElementById('mainView').classList.add('active');
        document.getElementById('archiveView').classList.remove('active');
        document.getElementById('toggleViewBtn').textContent = '📚 Arxivni ko\'rish';
      }
    });

    // Add question
    document.getElementById('addQuestionBtn').addEventListener('click', () => {
      const question = document.getElementById('questionInput').value.trim();
      if (!question) {
        alert('Savolni yozing!');
        return;
      }
      document.getElementById('passwordModal').classList.add('active');
    });

    // Confirm password
    document.getElementById('confirmPasswordBtn').addEventListener('click', () => {
      const password = document.getElementById('passwordInput').value;
      const question = document.getElementById('questionInput').value.trim();

      if (password !== OWNER_PASSWORD) {
        alert('Parol noto\'g\'ri!');
        document.getElementById('passwordInput').value = '';
        return;
      }

      const data = getData();
      
      // Archive current question if exists
      if (data.current && data.current.question) {
        data.archive.unshift({
          id: Date.now(),
          question: data.current.question,
          answers: data.current.answers || [],
          date: new Date().toLocaleDateString('uz-UZ')
        });
      }

      // Save new question
      const newCurrent = {
        question: question,
        answers: []
      };

      saveData(newCurrent, data.archive);

      document.getElementById('questionInput').value = '';
      document.getElementById('passwordInput').value = '';
      document.getElementById('passwordModal').classList.remove('active');
      
      alert('Savol muvaffaqiyatli joylandi!');
      render();
    });

    // Cancel password
    document.getElementById('cancelPasswordBtn').addEventListener('click', () => {
      document.getElementById('passwordInput').value = '';
      document.getElementById('passwordModal').classList.remove('active');
    });

    // Add answer
    document.getElementById('addAnswerBtn').addEventListener('click', () => {
      const answer = document.getElementById('answerInput').value.trim();
      if (!answer) {
        alert('Javobni yozing!');
        return;
      }

      const data = getData();
      if (!data.current || !data.current.question) {
        alert('Hozirda savol yo\'q!');
        return;
      }

      data.current.answers.unshift(answer);
      saveData(data.current, null);

      document.getElementById('answerInput').value = '';
      alert('Javob yuborildi!');
      render();
    });

    // Render main view
    function render() {
      const data = getData();
      
      // Render current question
      const questionDiv = document.getElementById('currentQuestion');
      if (data.current && data.current.question) {
        questionDiv.innerHTML = `<p class="question-display">${escapeHtml(data.current.question)}</p>`;
      } else {
        questionDiv.innerHTML = '<p class="empty-state">Hozirda savol yo\'q</p>';
      }

      // Render answers
      const answersDiv = document.getElementById('answersList');
      const answers = data.current ? data.current.answers || [] : [];
      
      document.getElementById('answerCount').textContent = `(${answers.length})`;

      if (answers.length === 0) {
        answersDiv.innerHTML = '<p class="empty-state">Hozircha javoblar yo\'q. Birinchi bo\'lib javob qoldiring!</p>';
      } else {
        answersDiv.innerHTML = answers.map(ans => 
          `<div class="answer">${escapeHtml(ans)}</div>`
        ).join('');
      }
    }

    // Render archive
    function renderArchive() {
      const data = getData();
      const archiveDiv = document.getElementById('archiveList');

      if (data.archive.length === 0) {
        archiveDiv.innerHTML = '<p class="empty-state">Arxivda hech narsa yo\'q</p>';
        return;
      }

      archiveDiv.innerHTML = data.archive.map(item => `
        <div class="archive-item">
          <div class="archive-header">
            <h3 class="archive-question">${escapeHtml(item.question)}</h3>
            <span class="archive-date">${item.date}</span>
          </div>
          <div class="archive-answers">
            <p class="archive-count">${item.answers.length} javob</p>
            ${item.answers.slice(0, 3).map(ans => 
              `<div class="archive-answer">${escapeHtml(ans)}</div>`
            ).join('')}
            ${item.answers.length > 3 ? 
              `<p class="archive-more">... va yana ${item.answers.length - 3} javob</p>` : 
              ''
            }
          </div>
        </div>
      `).join('');
    }

    // Enter key support for password
    document.getElementById('passwordInput').addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        document.getElementById('confirmPasswordBtn').click();
      }
    });

    // Initial render
    render();
  </script>
</body>
</html>
