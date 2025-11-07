// Configuration - Update these values with your actual links
const CONFIG = {
    resumePDF: "https://your-cloud-storage.com/rohit-dutta-resume.pdf", // Replace with your actual PDF link
    socialLinks: {
        linkedin: "https://linkedin.com/in/rohit-dutta",
        github: "https://github.com/rohitdutta",
        hackerrank: "https://hackerrank.com/rohitdutta"
    },
    contactInfo: {
        email: "rohitdutta2212rd@gmail.com",
        phone: "+91 9431792677",
        location: "Bengaluru, Karnataka, India"
    }
};

// DOM Elements
const hamburger = document.querySelector('.hamburger');
const navLinks = document.querySelector('.nav-links');
const themeToggle = document.getElementById('themeToggle');
const themeIcon = themeToggle.querySelector('i');
const resumeLink = document.getElementById('resumeLink');
const resumeModal = document.getElementById('resumeModal');
const closeModal = document.querySelector('.close');
const pdfViewer = document.getElementById('pdfViewer');
const downloadResume = document.getElementById('downloadResume');

// Initialize the application
function init() {
    setupEventListeners();
    setupResumeViewer();
    setupSocialLinks();
    setupScrollAnimations();
    loadThemePreference();
}

// Set up all event listeners
function setupEventListeners() {
    // Mobile Navigation Toggle
    hamburger.addEventListener('click', toggleMobileMenu);
    
    // Close mobile menu when clicking on a link
    document.querySelectorAll('.nav-links a').forEach(link => {
        link.addEventListener('click', closeMobileMenu);
    });
    
    // Theme Toggle
    themeToggle.addEventListener('click', toggleTheme);
    
    // Resume Modal
    resumeLink.addEventListener('click', openResumeModal);
    closeModal.addEventListener('click', closeResumeModal);
    downloadResume.addEventListener('click', downloadResumeFile);
    
    // Close modal when clicking outside
    window.addEventListener('click', (e) => {
        if (e.target === resumeModal) {
            closeResumeModal();
        }
    });
    
    // Header scroll effect
    window.addEventListener('scroll', handleHeaderScroll);
    
    // Smooth scrolling for anchor links
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', smoothScroll);
    });
}

// Setup Resume Viewer
function setupResumeViewer() {
    // Set the PDF source
    pdfViewer.src = CONFIG.resumePDF;
    
    // Set download link
    downloadResume.href = CONFIG.resumePDF;
    downloadResume.download = "Rohit-Dutta-Resume.pdf";
}

// Setup Social Links
function setupSocialLinks() {
    const linkedinLink = document.querySelector('a[href*="linkedin"]');
    const githubLink = document.querySelector('a[href*="github"]');
    const hackerrankLink = document.querySelector('a[href*="hackerrank"]');
    
    if (linkedinLink) linkedinLink.href = CONFIG.socialLinks.linkedin;
    if (githubLink) githubLink.href = CONFIG.socialLinks.github;
    if (hackerrankLink) hackerrankLink.href = CONFIG.socialLinks.hackerrank;
}

// Setup Scroll Animations
function setupScrollAnimations() {
    const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -50px 0px'
    };
    
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('visible');
                
                // Animate skill bars
                if (entry.target.classList.contains('about')) {
                    animateSkillBars();
                }
            }
        });
    }, observerOptions);
    
    // Observe elements for animation
    document.querySelectorAll('.project-card, .timeline-item').forEach(el => {
        observer.observe(el);
    });
}

// Mobile Menu Functions
function toggleMobileMenu() {
    navLinks.classList.toggle('active');
    hamburger.querySelector('i').classList.toggle('fa-times');
}

function closeMobileMenu() {
    navLinks.classList.remove('active');
    hamburger.querySelector('i').classList.remove('fa-times');
}

// Theme Functions
function toggleTheme() {
    document.body.classList.toggle('dark-theme');
    
    if (document.body.classList.contains('dark-theme')) {
        localStorage.setItem('theme', 'dark');
        themeIcon.classList.remove('fa-moon');
        themeIcon.classList.add('fa-sun');
    } else {
        localStorage.setItem('theme', 'light');
        themeIcon.classList.remove('fa-sun');
        themeIcon.classList.add('fa-moon');
    }
}

function loadThemePreference() {
    const currentTheme = localStorage.getItem('theme') || 'light';
    if (currentTheme === 'dark') {
        document.body.classList.add('dark-theme');
        themeIcon.classList.remove('fa-moon');
        themeIcon.classList.add('fa-sun');
    }
}

// Resume Modal Functions
function openResumeModal(e) {
    e.preventDefault();
    resumeModal.style.display = 'block';
    document.body.style.overflow = 'hidden'; // Prevent background scrolling
}

function closeResumeModal() {
    resumeModal.style.display = 'none';
    document.body.style.overflow = 'auto'; // Re-enable scrolling
}

function downloadResumeFile() {
    // This will trigger the download via the anchor tag's download attribute
    console.log('Downloading resume...');
}

// Scroll Functions
function handleHeaderScroll() {
    const header = document.querySelector('header');
    if (window.scrollY > 100) {
        header.style.background = 'var(--header-bg)';
        header.style.boxShadow = 'var(--shadow)';
    } else {
        header.style.background = 'var(--header-bg)';
        header.style.boxShadow = '0 2px 10px rgba(0, 0, 0, 0.1)';
    }
}

function smoothScroll(e) {
    e.preventDefault();
    
    const targetId = this.getAttribute('href');
    if (targetId === '#') return;
    
    const targetElement = document.querySelector(targetId);
    if (targetElement) {
        window.scrollTo({
            top: targetElement.offsetTop - 80,
            behavior: 'smooth'
        });
    }
}

// Animation Functions
function animateSkillBars() {
    document.querySelectorAll('.skill-progress').forEach(bar => {
        const width = bar.getAttribute('data-width');
        setTimeout(() => {
            bar.style.width = width + '%';
        }, 300);
    });
}

// Keyboard shortcuts
document.addEventListener('keydown', (e) => {
    // Close modal with Escape key
    if (e.key === 'Escape' && resumeModal.style.display === 'block') {
        closeResumeModal();
    }
    
    // Toggle theme with Ctrl/Cmd + T
    if ((e.ctrlKey || e.metaKey) && e.key === 't') {
        e.preventDefault();
        toggleTheme();
    }
});

// Initialize the application when DOM is loaded
document.addEventListener('DOMContentLoaded', init);

// Export configuration for easy updates
window.PORTFOLIO_CONFIG = CONFIG;