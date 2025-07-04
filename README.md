<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LumiKids - Instructivos en Video</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #b3e5fc;
        }
        
        .video-item.active {
            background-color: #fef3c7;
            border-left-color: #f59e0b;
        }
        
        .logo-container {
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }
        
        .logo-svg {
            width: 90px;
            height: 60px;
            flex-shrink: 0;
        }
        
        @media (max-width: 640px) {
            .logo-svg {
                width: 50px;
                height: 50px;
            }
        }
        
        .bg-baby-blue {
            background-color: #e1f5fe;
        }
        
        .text-dark-blue {
            color: #0d47a1;
        }

        /* Estilos para el área de drag & drop */
        .drop-zone {
            border: 2px dashed #cbd5e1;
            transition: all 0.3s ease;
        }
        
        .drop-zone.dragover {
            border-color: #3b82f6;
            background-color: #eff6ff;
        }

        /* Estilo para el modal */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }

        .modal.show {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Progress bar personalizada */
        .progress-bar {
            height: 8px;
            background-color: #e5e7eb;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background-color: #10b981;
            transition: width 0.3s ease;
        }
    </style>
</head>
<body class="bg-baby-blue text-slate-800">

    <!-- Header -->
    <header class="bg-white shadow-md">
        <nav class="container mx-auto px-6 py-4 flex justify-between items-center">
            <div class="logo-container">
                <img src="imagenes/Expoinnova.png" 
                     alt="LumiKids Logo" 
                     class="logo-svg"
                     loading="lazy">
            </div>
            <div class="flex space-x-8">
                <button id="upload-btn" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition duration-300 font-medium">
                    + Subir Video
                </button>
                <a href="#" class="text-slate-600 hover:text-amber-500 transition duration-300 font-medium">Nuestros Libros</a>
            </div>
        </nav>
    </header>

    <!-- Modal para Subir Videos -->
    <div id="upload-modal" class="modal">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-md mx-4">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-dark-blue">Subir Nuevo Video</h3>
                <button id="close-modal" class="text-gray-500 hover:text-gray-700 text-xl">&times;</button>
            </div>
            
            <form id="video-form">
                <!-- Título del video -->
                <div class="mb-4">
                    <label for="video-title-input" class="block text-sm font-medium text-gray-700 mb-2">Título del Video</label>
                    <input type="text" id="video-title-input" required 
                           class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                           placeholder="Ej: Aventura en el Océano">
                </div>
                
                <!-- Descripción -->
                <div class="mb-4">
                    <label for="video-desc-input" class="block text-sm font-medium text-gray-700 mb-2">Descripción</label>
                    <textarea id="video-desc-input" required rows="3"
                              class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                              placeholder="Describe el contenido del video..."></textarea>
                </div>
                
                <!-- Área de subida de archivo -->
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-2">Archivo de Video</label>
                    <div id="drop-zone" class="drop-zone p-6 text-center rounded-lg cursor-pointer">
                        <div id="drop-content">
                            <svg class="mx-auto h-12 w-12 text-gray-400 mb-4" stroke="currentColor" fill="none" viewBox="0 0 48 48">
                                <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                            <p class="text-gray-600">
                                <span class="font-medium text-blue-600">Haz clic para seleccionar</span>
                                o arrastra un archivo aquí
                            </p>
                            <p class="text-xs text-gray-500 mt-1">MP4, AVI, MOV hasta 100MB</p>
                        </div>
                        <div id="upload-progress" class="hidden">
                            <p class="text-sm text-gray-700 mb-2">Subiendo video...</p>
                            <div class="progress-bar">
                                <div id="progress-fill" class="progress-fill" style="width: 0%"></div>
                            </div>
                            <p id="progress-text" class="text-xs text-gray-500 mt-1">0%</p>
                        </div>
                    </div>
                    <input type="file" id="video-file" accept="video/*" class="hidden">
                </div>
                
                <!-- Botones -->
                <div class="flex justify-end space-x-3">
                    <button type="button" id="cancel-btn" class="px-4 py-2 text-gray-600 border border-gray-300 rounded-md hover:bg-gray-50">
                        Cancelar
                    </button>
                    <button type="submit" id="submit-btn" class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 disabled:opacity-50" disabled>
                        Subir Video
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Contenido Principal -->
    <main class="container mx-auto p-4 lg:p-8">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">

            <!-- Columna de Video Principal -->
            <div class="lg:col-span-2">
                <!-- Reproductor de Video -->
                <div class="bg-black rounded-lg shadow-lg overflow-hidden aspect-w-16 aspect-h-9 mb-4 relative">
                    <div id="video-loader" class="absolute inset-0 bg-black bg-opacity-75 flex items-center justify-center" style="display: none;">
                        <div class="text-center text-white">
                            <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-white mx-auto mb-4"></div>
                            <p>Cargando video...</p>
                        </div>
                    </div>
                    
                    <video id="main-video-player" 
                           controls 
                           class="w-full h-full"
                           style="min-height: 315px;">
                        Tu navegador no soporta el elemento de video.
                    </video>
                </div>

                <!-- Detalles del Video -->
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <h1 id="video-title" class="text-2xl lg:text-3xl font-bold mb-2 text-slate-900">Selecciona un video</h1>
                    <p id="video-description" class="text-slate-600 text-base">Elige un video de la lista para comenzar a verlo.</p>
                </div>

                <!-- Sección de Comentarios -->
                <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                    <h2 class="text-xl font-bold mb-4 text-dark-blue">Comentarios</h2>
                    <div class="space-y-4">
                        <div class="flex items-start space-x-3">
                            <img src="https://placehold.co/40x40/f59e0b/FFFFFF?text=A" alt="Avatar de Ana Pérez" class="rounded-full">
                            <div>
                                <p class="font-semibold text-dark-blue">Ana Pérez</p>
                                <p class="text-sm text-slate-600">¡Qué útil! No sabía cómo usar la página del océano. ¡Gracias!</p>
                            </div>
                        </div>
                        <div class="flex items-start space-x-3">
                            <img src="https://placehold.co/40x40/4ade80/FFFFFF?text=M" alt="Avatar de Marcos López" class="rounded-full">
                            <div>
                                <p class="font-semibold text-dark-blue">Marcos López</p>
                                <p class="text-sm text-slate-600">A mi hijo le encantó la parte de los animales de la granja. El video es muy claro.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Columna de Lista de Videos -->
            <div class="lg:col-span-1">
                <div class="bg-white p-4 rounded-lg shadow-md">
                    <h2 class="text-xl font-bold mb-4 border-b pb-2 text-dark-blue">Instructivos de Video</h2>
                    <div id="video-list" class="space-y-3">
                        <!-- Los videos se cargarán aquí dinámicamente -->
                    </div>
                </div>
            </div>

        </div>
    </main>
    
    <!-- Footer -->
    <footer class="bg-white mt-8 py-6">
        <div class="container mx-auto text-center text-slate-600">
            <p>&copy; 2025 LumiKids. Todos los derechos reservados.</p>
        </div>
    </footer>

    <script>
        // Sistema de gestión de videos
        class VideoManager {
            constructor() {
                this.videos = [
                    {
                        id: 1,
                        title: "Instructivo: Libro Sensorial 'Aventura en el Océano'",
                        description: "Descubre todas las actividades marinas de nuestro libro. Aprende a interactuar con el pulpo, los peces de colores y el tesoro escondido. Ideal para estimular la motricidad fina.",
                        src: "https://www.youtube.com/embed/SgZ1f4-I22I",
                        thumbnail: "https://images.unsplash.com/photo-1559827260-dc66d52bef19?w=120&h=68&fit=crop&crop=center",
                        type: "youtube"
                    },
                    {
                        id: 2,
                        title: "Instructivo: Libro Sensorial 'La Granja Divertida'",
                        description: "Te guiamos por cada página de nuestra granja. Aprende a usar el granero, contar los pollitos y sentir las texturas de la lana de la oveja. ¡Una experiencia de aprendizaje y diversión!",
                        src: "https://www.youtube.com/embed/u_yIGGhub_s",
                        thumbnail: "https://images.unsplash.com/photo-1500595046743-cd271d694d30?w=120&h=68&fit=crop&crop=center",
                        type: "youtube"
                    },
                    {
                        id: 3,
                        title: "Instructivo: Libro Sensorial 'Exploradores del Espacio'",
                        description: "Viaja por el cosmos con nuestro libro del espacio. Descubre cómo mover los planetas, abrochar el cohete y sentir las estrellas brillantes. Perfecto para pequeños astronautas.",
                        src: "https://www.youtube.com/embed/Z_6v_vn_60M",
                        thumbnail: "https://images.unsplash.com/photo-1446776877081-d282a0f896e2?w=120&h=68&fit=crop&crop=center",
                        type: "youtube"
                    },
                    {
                        id: 4,
                        title: "Instructivo: Libro Sensorial 'El Bosque Mágico'",
                        description: "Sumérgete en un bosque encantado. Sigue las instrucciones para ayudar al erizo a encontrar su camino, abrir la flor sorpresa y descubrir los sonidos del bosque.",
                        src: "https://www.youtube.com/embed/5LI_a_Aa4j4",
                        thumbnail: "https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=120&h=68&fit=crop&crop=center",
                        type: "youtube"
                    }
                ];
                this.currentVideoId = null;
                this.init();
            }

            init() {
                this.bindEvents();
                this.renderVideoList();
                if (this.videos.length > 0) {
                    this.loadVideo(this.videos[0]);
                }
            }

            bindEvents() {
                // Modal events
                document.getElementById('upload-btn').addEventListener('click', () => this.openModal());
                document.getElementById('close-modal').addEventListener('click', () => this.closeModal());
                document.getElementById('cancel-btn').addEventListener('click', () => this.closeModal());
                
                // File upload events
                const dropZone = document.getElementById('drop-zone');
                const fileInput = document.getElementById('video-file');
                
                dropZone.addEventListener('click', () => fileInput.click());
                dropZone.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    dropZone.classList.add('dragover');
                });
                dropZone.addEventListener('dragleave', () => {
                    dropZone.classList.remove('dragover');
                });
                dropZone.addEventListener('drop', (e) => {
                    e.preventDefault();
                    dropZone.classList.remove('dragover');
                    const files = e.dataTransfer.files;
                    if (files.length > 0) {
                        this.handleFileSelect(files[0]);
                    }
                });
                
                fileInput.addEventListener('change', (e) => {
                    if (e.target.files.length > 0) {
                        this.handleFileSelect(e.target.files[0]);
                    }
                });

                // Form submit
                document.getElementById('video-form').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.submitVideo();
                });
            }

            openModal() {
                document.getElementById('upload-modal').classList.add('show');
                this.resetForm();
            }

            closeModal() {
                document.getElementById('upload-modal').classList.remove('show');
                this.resetForm();
            }

            resetForm() {
                document.getElementById('video-form').reset();
                document.getElementById('drop-content').style.display = 'block';
                document.getElementById('upload-progress').classList.add('hidden');
                document.getElementById('submit-btn').disabled = true;
                this.selectedFile = null;
            }

            handleFileSelect(file) {
                // Validar tipo de archivo
                if (!file.type.startsWith('video/')) {
                    alert('Por favor selecciona un archivo de video válido.');
                    return;
                }

                // Validar tamaño (100MB = 100 * 1024 * 1024 bytes)
                if (file.size > 100 * 1024 * 1024) {
                    alert('El archivo es demasiado grande. El tamaño máximo es 100MB.');
                    return;
                }

                this.selectedFile = file;
                document.getElementById('submit-btn').disabled = false;
                
                // Mostrar información del archivo
                document.getElementById('drop-content').innerHTML = `
                    <div class="text-center">
                        <svg class="mx-auto h-12 w-12 text-green-500 mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                        </svg>
                        <p class="text-sm font-medium text-gray-900">${file.name}</p>
                        <p class="text-xs text-gray-500">${(file.size / (1024 * 1024)).toFixed(2)} MB</p>
                    </div>
                `;
            }

            simulateUpload() {
                document.getElementById('drop-content').style.display = 'none';
                document.getElementById('upload-progress').classList.remove('hidden');
                
                let progress = 0;
                const interval = setInterval(() => {
                    progress += Math.random() * 15;
                    if (progress > 100) progress = 100;
                    
                    document.getElementById('progress-fill').style.width = `${progress}%`;
                    document.getElementById('progress-text').textContent = `${Math.round(progress)}%`;
                    
                    if (progress >= 100) {
                        clearInterval(interval);
                        setTimeout(() => {
                            this.finishUpload();
                        }, 500);
                    }
                }, 200);
            }

            finishUpload() {
                const title = document.getElementById('video-title-input').value;
                const description = document.getElementById('video-desc-input').value;
                
                // Crear URL temporal para el video
                const videoUrl = URL.createObjectURL(this.selectedFile);
                
                // Crear miniatura del video
                this.createVideoThumbnail(this.selectedFile).then(thumbnailUrl => {
                    const newVideo = {
                        id: Date.now(),
                        title: title,
                        description: description,
                        src: videoUrl,
                        thumbnail: thumbnailUrl || "https://images.unsplash.com/photo-1611162617474-5b21e879e113?w=120&h=68&fit=crop&crop=center",
                        type: "local"
                    };

                    this.videos.unshift(newVideo);
                    this.renderVideoList();
                    this.closeModal();
                    
                    // Mostrar mensaje de éxito
                    alert('¡Video subido exitosamente!');
                });
            }

            submitVideo() {
                if (!this.selectedFile) {
                    alert('Por favor selecciona un archivo de video.');
                    return;
                }

                const title = document.getElementById('video-title-input').value;
                const description = document.getElementById('video-desc-input').value;

                if (!title.trim() || !description.trim()) {
                    alert('Por favor completa todos los campos.');
                    return;
                }

                this.simulateUpload();
            }

            renderVideoList() {
                const videoList = document.getElementById('video-list');
                videoList.innerHTML = '';

                this.videos.forEach(video => {
                    const videoItem = document.createElement('div');
                    videoItem.className = 'video-item flex items-center space-x-3 p-2 rounded-lg hover:bg-slate-50 transition duration-300 border-l-4 border-transparent cursor-pointer';
                    videoItem.innerHTML = `
                        <img src="${video.thumbnail}" alt="Miniatura de ${video.title}" class="rounded-md w-28 h-16 object-cover" loading="lazy">
                        <div class="flex-1">
                            <h3 class="font-semibold text-sm leading-tight text-dark-blue">${video.title}</h3>
                            <p class="text-xs text-slate-500">LumiKids</p>
                            ${video.type === 'local' ? '<span class="text-xs bg-green-100 text-green-800 px-2 py-1 rounded mt-1 inline-block">Subido</span>' : ''}
                        </div>
                        ${video.type === 'local' ? `
                            <button class="delete-video-btn text-red-500 hover:text-red-700 p-1 rounded-full hover:bg-red-50 transition duration-200" 
                                    data-video-id="${video.id}" 
                                    title="Eliminar video">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path>
                                </svg>
                            </button>
                        ` : ''}
                    `;
                    
                    videoItem.addEventListener('click', (e) => {
                        // No cargar video si se hace clic en el botón de eliminar
                        if (!e.target.closest('.delete-video-btn')) {
                            this.loadVideo(video);
                        }
                    });
                    
                    // Agregar event listener para el botón de eliminar
                    const deleteBtn = videoItem.querySelector('.delete-video-btn');
                    if (deleteBtn) {
                        deleteBtn.addEventListener('click', (e) => {
                            e.stopPropagation();
                            this.confirmDeleteVideo(video.id);
                        });
                    }
                    
                    videoList.appendChild(videoItem);
                });
            }

            loadVideo(video) {
                const mainVideoPlayer = document.getElementById('main-video-player');
                const videoTitle = document.getElementById('video-title');
                const videoDescription = document.getElementById('video-description');
                const videoLoader = document.getElementById('video-loader');

                // Mostrar loading
                videoLoader.style.display = 'flex';
                mainVideoPlayer.style.opacity = '0.5';

                setTimeout(() => {
                    // Actualizar contenido
                    if (video.type === 'youtube') {
                        // Para videos de YouTube, crear un iframe
                        mainVideoPlayer.style.display = 'none';
                        let iframe = document.getElementById('youtube-iframe');
                        if (!iframe) {
                            iframe = document.createElement('iframe');
                            iframe.id = 'youtube-iframe';
                            iframe.className = 'w-full h-full';
                            iframe.style.minHeight = '315px';
                            iframe.frameBorder = '0';
                            iframe.allow = 'accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture';
                            iframe.allowFullscreen = true;
                            mainVideoPlayer.parentNode.appendChild(iframe);
                        }
                        iframe.src = video.src;
                        iframe.style.display = 'block';
                    } else {
                        // Para videos locales, usar el elemento video
                        const iframe = document.getElementById('youtube-iframe');
                        if (iframe) iframe.style.display = 'none';
                        mainVideoPlayer.src = video.src;
                        mainVideoPlayer.style.display = 'block';
                    }

                    videoTitle.textContent = video.title;
                    videoDescription.textContent = video.description;

                    // Ocultar loading
                    videoLoader.style.display = 'none';
                    mainVideoPlayer.style.opacity = '1';

                    // Actualizar estado activo
                    document.querySelectorAll('.video-item').forEach(item => {
                        item.classList.remove('active');
                    });
                    
                    const currentItem = Array.from(document.querySelectorAll('.video-item')).find(item => 
                        item.querySelector('h3').textContent === video.title
                    );
                    if (currentItem) {
                        currentItem.classList.add('active');
                    }

                    this.currentVideoId = video.id;
                }, 800);
            }

            createVideoThumbnail(videoFile) {
                return new Promise((resolve) => {
                    const video = document.createElement('video');
                    const canvas = document.createElement('canvas');
                    const ctx = canvas.getContext('2d');
                    
                    video.addEventListener('loadedmetadata', () => {
                        // Configurar el canvas con las dimensiones deseadas
                        canvas.width = 120;
                        canvas.height = 68;
                        
                        // Ir al segundo 1 del video para obtener una buena imagen
                        video.currentTime = 1;
                    });
                    
                    video.addEventListener('seeked', () => {
                        // Dibujar el frame del video en el canvas
                        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
                        
                        // Convertir a blob y crear URL
                        canvas.toBlob((blob) => {
                            if (blob) {
                                resolve(URL.createObjectURL(blob));
                            } else {
                                resolve(null);
                            }
                        }, 'image/jpeg', 0.8);
                    });
                    
                    video.addEventListener('error', () => {
                        resolve(null);
                    });
                    
                    video.src = URL.createObjectURL(videoFile);
                });
            }

            confirmDeleteVideo(videoId) {
                if (confirm('¿Estás seguro de que quieres eliminar este video?')) {
                    this.deleteVideo(videoId);
                }
            }

            deleteVideo(videoId) {
                const videoIndex = this.videos.findIndex(v => v.id === videoId);
                if (videoIndex === -1) return;

                const video = this.videos[videoIndex];
                
                // Liberar URLs de objeto para evitar memory leaks
                if (video.type === 'local') {
                    if (video.src.startsWith('blob:')) {
                        URL.revokeObjectURL(video.src);
                    }
                    if (video.thumbnail.startsWith('blob:')) {
                        URL.revokeObjectURL(video.thumbnail);
                    }
                }
                
                // Eliminar del array
                this.videos.splice(videoIndex, 1);
                
                // Re-renderizar la lista
                this.renderVideoList();
                
                // Si el video eliminado era el que se estaba reproduciendo, cargar otro
                if (this.currentVideoId === videoId) {
                    if (this.videos.length > 0) {
                        this.loadVideo(this.videos[0]);
                    } else {
                        // Si no hay más videos, limpiar el reproductor
                        const mainVideoPlayer = document.getElementById('main-video-player');
                        const videoTitle = document.getElementById('video-title');
                        const videoDescription = document.getElementById('video-description');
                        const iframe = document.getElementById('youtube-iframe');
                        
                        mainVideoPlayer.src = '';
                        mainVideoPlayer.style.display = 'block';
                        if (iframe) iframe.style.display = 'none';
                        
                        videoTitle.textContent = 'No hay videos disponibles';
                        videoDescription.textContent = 'Sube un video o selecciona uno de la lista para comenzar.';
                        
                        this.currentVideoId = null;
                    }
                }
                
                alert('Video eliminado exitosamente.');
            }
        }

        // Inicializar la aplicación cuando el DOM esté listo
        document.addEventListener('DOMContentLoaded', function() {
            new VideoManager();
        });
    </script>
</body>
</html>
