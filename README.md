<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Note Sharing App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(to right, #4a00e0, #8e2de2);
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1rem;
            opacity: 0.9;
        }
        
        .main-content {
            padding: 25px;
        }
        
        .note-form {
            background-color: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 30px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #495057;
        }
        
        input, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ced4da;
            border-radius: 5px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }
        
        input:focus, textarea:focus {
            outline: none;
            border-color: #6a11cb;
            box-shadow: 0 0 0 3px rgba(106, 17, 203, 0.1);
        }
        
        textarea {
            min-height: 120px;
            resize: vertical;
        }
        
        button {
            background: linear-gradient(to right, #4a00e0, #8e2de2);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .notes-section h2 {
            color: #495057;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e9ecef;
        }
        
        .note {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            border-left: 4px solid #8e2de2;
        }
        
        .note-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .note-author {
            font-weight: 600;
            color: #4a00e0;
        }
        
        .note-date {
            color: #6c757d;
            font-size: 0.9rem;
        }
        
        .note-content {
            margin-bottom: 15px;
            line-height: 1.5;
        }
        
        .replies {
            margin-top: 15px;
            padding-left: 20px;
            border-left: 2px solid #e9ecef;
        }
        
        .reply {
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        
        .reply-form {
            margin-top: 15px;
            display: none;
        }
        
        .reply-btn {
            background: linear-gradient(to right, #00b09b, #96c93d);
            padding: 8px 15px;
            font-size: 0.9rem;
        }
        
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: #6c757d;
        }
        
        .empty-state i {
            font-size: 3rem;
            margin-bottom: 15px;
            color: #adb5bd;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            color: #6c757d;
            font-size: 0.9rem;
            border-top: 1px solid #e9ecef;
        }
        
        @media (max-width: 600px) {
            .container {
                border-radius: 10px;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .main-content {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Note Sharing App</h1>
            <p class="subtitle">Write notes and share with others to get their replies</p>
        </header>
        
        <div class="main-content">
            <div class="note-form">
                <h2>Write a New Note</h2>
                <form id="noteForm">
                    <div class="form-group">
                        <label for="authorName">Your Name</label>
                        <input type="text" id="authorName" placeholder="Enter your name" required>
                    </div>
                    <div class="form-group">
                        <label for="noteContent">Your Note</label>
                        <textarea id="noteContent" placeholder="Write your note here..." required></textarea>
                    </div>
                    <button type="submit">Post Note</button>
                </form>
            </div>
            
            <div class="notes-section">
                <h2>Shared Notes</h2>
                <div id="notesContainer">
                    <!-- Notes will be displayed here -->
                    <div class="empty-state">
                        <i>📝</i>
                        <p>No notes yet. Be the first to share a note!</p>
                    </div>
                </div>
            </div>
        </div>
        
        <footer>
            <p>Note Sharing App &copy; 2023 | Share your thoughts and get feedback</p>
        </footer>
    </div>

    <script>
        // Sample initial notes data
        let notes = [
            {
                id: 1,
                author: "Alex Johnson",
                content: "This is a great idea for sharing thoughts and getting feedback! Looking forward to seeing what others think.",
                date: "2023-10-15",
                replies: [
                    { author: "Sam Wilson", content: "I agree! This could be really useful for team collaboration.", date: "2023-10-16" },
                    { author: "Taylor Smith", content: "Love the simplicity of this app. Easy to use and effective.", date: "2023-10-17" }
                ]
            },
            {
                id: 2,
                author: "Maria Garcia",
                content: "Has anyone tried the new project management tool? I'd love to hear your experiences before we decide to adopt it.",
                date: "2023-10-18",
                replies: [
                    { author: "David Lee", content: "We've been using it for a month now. The learning curve is a bit steep but it's powerful once you get the hang of it.", date: "2023-10-19" }
                ]
            }
        ];

        // DOM elements
        const noteForm = document.getElementById('noteForm');
        const notesContainer = document.getElementById('notesContainer');
        const authorNameInput = document.getElementById('authorName');
        const noteContentInput = document.getElementById('noteContent');

        // Display notes when page loads
        document.addEventListener('DOMContentLoaded', displayNotes);

        // Handle form submission
        noteForm.addEventListener('submit', function(e) {
            e.preventDefault();
            
            const author = authorNameInput.value.trim();
            const content = noteContentInput.value.trim();
            
            if (author && content) {
                addNote(author, content);
                authorNameInput.value = '';
                noteContentInput.value = '';
            }
        });

        // Function to add a new note
        function addNote(author, content) {
            const newNote = {
                id: Date.now(), // Simple ID generation
                author: author,
                content: content,
                date: new Date().toISOString().split('T')[0], // YYYY-MM-DD format
                replies: []
            };
            
            notes.unshift(newNote); // Add to beginning of array
            displayNotes();
        }

        // Function to add a reply to a note
        function addReply(noteId, author, content) {
            const note = notes.find(note => note.id === noteId);
            if (note) {
                const newReply = {
                    author: author,
                    content: content,
                    date: new Date().toISOString().split('T')[0] // YYYY-MM-DD format
                };
                
                note.replies.push(newReply);
                displayNotes();
            }
        }

        // Function to display all notes
        function displayNotes() {
            if (notes.length === 0) {
                notesContainer.innerHTML = `
                    <div class="empty-state">
                        <i>📝</i>
                        <p>No notes yet. Be the first to share a note!</p>
                    </div>
                `;
                return;
            }
            
            notesContainer.innerHTML = '';
            
            notes.forEach(note => {
                const noteElement = document.createElement('div');
                noteElement.className = 'note';
                noteElement.innerHTML = `
                    <div class="note-header">
                        <span class="note-author">${note.author}</span>
                        <span class="note-date">${formatDate(note.date)}</span>
                    </div>
                    <div class="note-content">${note.content}</div>
                    <button class="reply-btn" data-note-id="${note.id}">Reply</button>
                    
                    <div class="replies">
                        ${note.replies.map(reply => `
                            <div class="reply">
                                <div class="note-header">
                                    <span class="note-author">${reply.author}</span>
                                    <span class="note-date">${formatDate(reply.date)}</span>
                                </div>
                                <div class="note-content">${reply.content}</div>
                            </div>
                        `).join('')}
                    </div>
                    
                    <form class="reply-form" id="replyForm-${note.id}">
                        <div class="form-group">
                            <input type="text" placeholder="Your name" class="reply-author" required>
                        </div>
                        <div class="form-group">
                            <textarea placeholder="Your reply..." class="reply-content" required></textarea>
                        </div>
                        <button type="submit">Post Reply</button>
                    </form>
                `;
                
                notesContainer.appendChild(noteElement);
            });
            
            // Add event listeners to reply buttons
            document.querySelectorAll('.reply-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const noteId = parseInt(this.getAttribute('data-note-id'));
                    const replyForm = document.getElementById(`replyForm-${noteId}`);
                    replyForm.style.display = replyForm.style.display === 'block' ? 'none' : 'block';
                });
            });
            
            // Add event listeners to reply forms
            document.querySelectorAll('form[id^="replyForm-"]').forEach(form => {
                form.addEventListener('submit', function(e) {
                    e.preventDefault();
                    const noteId = parseInt(this.id.split('-')[1]);
                    const authorInput = this.querySelector('.reply-author');
                    const contentInput = this.querySelector('.reply-content');
                    
                    const author = authorInput.value.trim();
                    const content = contentInput.value.trim();
                    
                    if (author && content) {
                        addReply(noteId, author, content);
                        authorInput.value = '';
                        contentInput.value = '';
                        this.style.display = 'none';
                    }
                });
            });
        }

        // Helper function to format date
        function formatDate(dateString) {
            const options = { year: 'numeric', month: 'short', day: 'numeric' };
            return new Date(dateString).toLocaleDateString(undefined, options);
        }
    </script>
</body>
</html>
