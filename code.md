import React, { useEffect, useRef, useState } from 'react';
import { Rocket, BookOpen, Users, Video, MapPin, Sparkles, ChevronRight, Telescope, Megaphone, MessageSquare } from 'lucide-react';

// --- Cinematic Scroll Reveal Component ---
const Reveal = ({ children, delay = 0, className = "" }) => {
  const [isVisible, setIsVisible] = useState(false);
  const ref = useRef(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true);
          observer.unobserve(entry.target); // Only animate once
        }
      },
      { threshold: 0.1, rootMargin: "0px 0px -50px 0px" }
    );

    if (ref.current) {
      observer.observe(ref.current);
    }

    return () => observer.disconnect();
  }, []);

  return (
    <div
      ref={ref}
      className={`transition-all duration-1000 ease-out ${
        isVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-12'
      } ${className}`}
      style={{ transitionDelay: `${delay}ms` }}
    >
      {children}
    </div>
  );
};

// --- Dynamic Space Starfield Background ---
const Starfield = () => {
  const canvasRef = useRef(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    let animationFrameId;
    let stars = [];

    const resize = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      initStars();
    };

    const initStars = () => {
      stars = [];
      const numStars = Math.floor((window.innerWidth * window.innerHeight) / 2000);
      for (let i = 0; i < numStars; i++) {
        stars.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          radius: Math.random() * 1.5,
          vx: Math.floor(Math.random() * 50) - 25,
          vy: Math.floor(Math.random() * 50) - 25,
          alpha: Math.random(),
          speed: Math.random() * 0.01,
        });
      }
    };

    const draw = () => {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      
      // Vintage Dark Background (No Gradients)
      ctx.fillStyle = '#0f1115';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Draw Stars
      stars.forEach((star) => {
        star.alpha += star.speed;
        if (star.alpha > 1 || star.alpha < 0) star.speed = -star.speed;

        // Slow drift
        star.y -= 0.1;
        if (star.y < 0) star.y = canvas.height;

        ctx.beginPath();
        ctx.arc(star.x, star.y, star.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(200, 200, 220, ${Math.abs(star.alpha)})`; // Classic white/silver stars
        ctx.fill();
      });

      animationFrameId = requestAnimationFrame(draw);
    };

    window.addEventListener('resize', resize);
    resize();
    draw();

    return () => {
      window.removeEventListener('resize', resize);
      cancelAnimationFrame(animationFrameId);
    };
  }, []);

  return <canvas ref={canvasRef} className="fixed inset-0 z-0 pointer-events-none" />;
};

// --- Main Application Component ---
export default function App() {
  const announcementsLink = "https://whatsapp.com/channel/0029VbCKl87GzzKPcPAnTV1B";
  const groupLink = "https://discord.gg/B2uBxAzzeD";

  return (
    <div className="min-h-screen text-slate-300 font-serif selection:bg-slate-700/50 overflow-x-hidden" style={{ fontFamily: "'Playfair Display', serif" }}>
      {/* Import elegant vintage fonts */}
      <style>
        {`
          @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400;1,600&display=swap');
          @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
          .cursive-text {
            font-family: 'Playfair Display', serif;
            font-style: italic;
          }
          .pixel-font {
            font-family: 'Press Start 2P', monospace;
          }
        `}
      </style>
      <Starfield />

      {/* Top Left Header */}
      <header className="absolute top-0 left-0 w-full p-6 z-20 flex justify-start">
        <span className="text-xs md:text-sm tracking-widest text-slate-400 uppercase">
          Young Physics & Astrophysics Enthusiasts Club
        </span>
      </header>

      {/* Main Content Container */}
      <main className="relative z-10 container mx-auto px-6 py-12 max-w-5xl flex flex-col items-center justify-center min-h-screen">
        
        {/* --- 1. HERO SECTION --- */}
        <section className="min-h-[50vh] flex flex-col items-center justify-center text-center w-full mt-16">
          <Reveal>
            <h1 className="text-4xl md:text-5xl lg:text-6xl font-normal tracking-tight mb-6 leading-tight text-slate-100 flex flex-col items-center gap-4">
              <span>Welcome to <span className="text-blue-500 pixel-font text-3xl md:text-4xl lg:text-5xl pr-2 align-middle">YPAE!</span></span>
              <span className="text-2xl md:text-3xl lg:text-4xl text-slate-300 font-normal mt-2 block">The right place to find the right people.</span>
            </h1>
          </Reveal>

          <Reveal delay={150}>
            <p className="text-base text-slate-400 max-w-xl mx-auto mb-10 leading-relaxed">
              A community built for the curious. Dive into the cosmos, unravel quantum mysteries, and connect with fellow stargazers.
            </p>
          </Reveal>

          <Reveal delay={300}>
            <div className="flex flex-col md:flex-row gap-4 max-w-4xl mx-auto w-full text-left">
              {/* Announcements Box */}
              <a 
                href={announcementsLink}
                target="_blank"
                rel="noopener noreferrer"
                className="group flex-1 flex flex-col p-6 border border-blue-500/50 hover:border-blue-400 bg-[#161821]/50 hover:bg-blue-500/10 transition-all duration-300 rounded-sm"
              >
                <div className="flex items-center gap-3 mb-3">
                  <Megaphone className="w-5 h-5 text-blue-500 group-hover:-rotate-12 transition-transform duration-300" />
                  <span className="text-blue-500 uppercase tracking-widest text-sm font-semibold font-sans">Join Announcements Channel</span>
                </div>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">
                  Get news, study resources, and opportunities related to Physics, Astronomy, and Astrophysics.
                </p>
              </a>

              {/* Group Box */}
              <a 
                href={groupLink}
                target="_blank"
                rel="noopener noreferrer"
                className="group flex-1 flex flex-col p-6 border border-blue-500/50 hover:border-blue-400 bg-[#161821]/50 hover:bg-blue-500/10 transition-all duration-300 rounded-sm"
              >
                <div className="flex items-center gap-3 mb-3">
                  <MessageSquare className="w-5 h-5 text-blue-500 group-hover:scale-110 transition-transform duration-300" />
                  <span className="text-blue-500 uppercase tracking-widest text-sm font-semibold font-sans">Join Group</span>
                </div>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">
                  Join interesting conversations with fellow like-minded people, form teams for research, hackathons and more.
                </p>
              </a>
            </div>
          </Reveal>
        </section>

        {/* --- 2. MISSION SECTION --- */}
        <section className="w-full pt-16 pb-8">
          <Reveal>
            <div className="mb-10 border-b border-white/10 pb-4 text-left">
              <h2 className="text-3xl font-normal tracking-tight mb-2 text-slate-100">Our Mission</h2>
            </div>
            <p className="text-xl md:text-2xl text-slate-200 leading-relaxed cursive-text text-left max-w-4xl">
              To create a global platform for young physics, astrophysics, and astronomy enthusiasts.
            </p>
          </Reveal>
        </section>

        {/* --- 3. WHAT TO EXPECT SECTION --- */}
        <section className="w-full pt-8 pb-16 relative">
          <Reveal>
            <div className="flex flex-col md:flex-row justify-between items-end mb-10 gap-4 border-b border-white/10 pb-4">
              <div>
                <h2 className="text-3xl font-normal tracking-tight mb-2 text-slate-100">What to expect</h2>
                <p className="text-sm text-slate-400">Everything you need to fuel your cosmic curiosity.</p>
              </div>
              
              {/* "All for free" Badge */}
              <div className="px-4 py-1 border border-slate-500 rounded-sm text-slate-300 tracking-widest uppercase text-xs">
                All for free
              </div>
            </div>
          </Reveal>

          {/* Bento Box Grid */}
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            
            <Reveal delay={100} className="h-full">
              <div className="group h-full bg-[#161821]/50 border border-white/10 hover:border-slate-400 rounded-sm p-6 transition-all duration-300">
                <BookOpen className="w-6 h-6 text-slate-300 mb-4" />
                <h3 className="text-xl font-normal mb-2 text-slate-100">Free Resources</h3>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">Access a curated library of resources to teach yourself various complex concepts of physics and astronomy at your own pace.</p>
              </div>
            </Reveal>

            <Reveal delay={200} className="h-full">
              <div className="group h-full bg-[#161821]/50 border border-white/10 hover:border-slate-400 rounded-sm p-6 transition-all duration-300">
                <Users className="w-6 h-6 text-slate-300 mb-4" />
                <h3 className="text-xl font-normal mb-2 text-slate-100">Community Chats</h3>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">Engage in deep chats, theoretical debates, and build lasting connections with like-minded peers across the globe.</p>
              </div>
            </Reveal>

            <Reveal delay={300} className="h-full">
              <div className="group h-full bg-[#161821]/50 border border-white/10 hover:border-slate-400 rounded-sm p-6 transition-all duration-300">
                <Video className="w-6 h-6 text-slate-300 mb-4" />
                <h3 className="text-xl font-normal mb-2 text-slate-100">Online Events</h3>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">Join interactive workshops, guest lectures from experts, and virtual rocket launch watch parties.</p>
              </div>
            </Reveal>

            <Reveal delay={400} className="h-full">
              <div className="group h-full bg-[#161821]/50 border border-white/10 hover:border-slate-400 rounded-sm p-6 transition-all duration-300">
                <MapPin className="w-6 h-6 text-slate-300 mb-4" />
                <h3 className="text-xl font-normal mb-2 text-slate-100">Future Offline Meets</h3>
                <p className="text-sm text-slate-400 leading-relaxed font-sans">Look forward to potential local meetups, stargazing trips, and offline astronomy camps in the future.</p>
              </div>
            </Reveal>

          </div>
        </section>
        
        {/* --- 4. BOTTOM CTA & FOOTER --- */}
        <section className="w-full py-16 text-center mt-6">
          <Reveal>
            <h2 className="text-2xl font-normal mb-8 text-slate-200">Ready to explore the universe?</h2>
            <div className="flex flex-col sm:flex-row gap-4 justify-center items-center">
              <a 
                href={announcementsLink}
                target="_blank"
                rel="noopener noreferrer"
                className="group inline-flex items-center gap-2 px-6 py-3 border border-blue-500/50 hover:border-blue-400 hover:bg-blue-500/10 transition-all duration-300 rounded-sm text-sm uppercase tracking-widest text-blue-500"
              >
                <Megaphone className="w-4 h-4 group-hover:-rotate-12 transition-transform" /> Announcements Channel
              </a>
              <a 
                href={groupLink}
                target="_blank"
                rel="noopener noreferrer"
                className="group inline-flex items-center gap-2 px-6 py-3 border border-blue-500/50 hover:border-blue-400 hover:bg-blue-500/10 transition-all duration-300 rounded-sm text-sm uppercase tracking-widest text-blue-500"
              >
                <MessageSquare className="w-4 h-4 group-hover:scale-110 transition-transform" /> Join Group
              </a>
            </div>
          </Reveal>
        </section>

        <footer className="w-full py-6 text-center text-slate-500 text-xs tracking-widest uppercase border-t border-white/10">
          <p>Built by Yuvika Varun</p>
        </footer>

      </main>
    </div>
  );
}
