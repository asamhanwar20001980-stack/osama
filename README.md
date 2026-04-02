# osama
My video editing portfolio website
[PortfolioPage.tsx](https://github.com/user-attachments/files/26446659/PortfolioPage.tsx)
import { useState, useEffect, useRef } from "react";
import BeforeAfterSlider from "@/components/BeforeAfterSlider";

const videos = [
  { id: 1, title: "ريل إبداعي #1", category: "ريلز", url: "https://www.instagram.com/reel/DJhUqrXtNt1/embed/", link: "https://www.instagram.com/reel/DJhUqrXtNt1/" },
  { id: 2, title: "ريل إبداعي #2", category: "ريلز", url: "https://www.instagram.com/reel/DP24yfIAo03/embed/", link: "https://www.instagram.com/reel/DP24yfIAo03/" },
  { id: 3, title: "ريل إبداعي #3", category: "ريلز", url: "https://www.instagram.com/reel/DHWk_LLOTcB/embed/", link: "https://www.instagram.com/reel/DHWk_LLOTcB/" },
  { id: 4, title: "ريل إبداعي #4", category: "ريلز", url: "https://www.instagram.com/reel/DWjyocDiSgx/embed/", link: "https://www.instagram.com/reel/DWjyocDiSgx/" },
  { id: 5, title: "ريل إبداعي #5", category: "ريلز", url: "https://www.instagram.com/reel/DWotvsmi3tf/embed/", link: "https://www.instagram.com/reel/DWotvsmi3tf/" },
  { id: 6, title: "ريل إبداعي #6", category: "ريلز", url: "https://www.instagram.com/reel/DJHnzVJNlPY/embed/", link: "https://www.instagram.com/reel/DJHnzVJNlPY/" },
];

const PRICING = {
  reels: { label: "مونتاج ريلز", base: 198, durations: [{ label: "15 ثانية", mult: 1 }, { label: "30 ثانية", mult: 1.4 }, { label: "60 ثانية", mult: 1.8 }] },
  ads: { label: "فيديو إعلاني", base: 350, durations: [{ label: "30 ثانية", mult: 1 }, { label: "60 ثانية", mult: 1.5 }, { label: "دقيقتان", mult: 2.2 }] },
  corporate: { label: "فيديو مؤسسي", base: 600, durations: [{ label: "دقيقة", mult: 1 }, { label: "3 دقائق", mult: 1.8 }, { label: "5 دقائق", mult: 2.5 }, { label: "10 دقائق", mult: 4 }] },
};

const packages = [
  {
    id: "pkg-reels",
    name: "باقة الريلز",
    price: "399",
    period: "شهرياً",
    color: "from-violet-500/20 to-violet-500/5",
    border: "border-violet-500/30",
    highlight: false,
    features: ["3 ريلز شهرياً", "مؤثرات بصرية احترافية", "موسيقى متزامنة", "تسليم خلال 48 ساعة", "تعديل مجاني"],
  },
  {
    id: "pkg-ads",
    name: "باقة الإعلانات",
    price: "799",
    period: "شهرياً",
    color: "from-primary/25 to-primary/5",
    border: "border-primary/40",
    highlight: true,
    features: ["2 إعلان تجاري", "سيناريو وإخراج", "موشن جرافيك", "تسليم خلال 72 ساعة", "3 تعديلات مجانية", "ألوان العلامة التجارية"],
  },
  {
    id: "pkg-corporate",
    name: "باقة المؤسسات",
    price: "1499",
    period: "شهرياً",
    color: "from-sky-500/20 to-sky-500/5",
    border: "border-sky-500/30",
    highlight: false,
    features: ["فيديو مؤسسي كامل", "تصميم هوية بصرية", "موشن جرافيك متقدم", "تسليم خلال 5 أيام", "تعديلات غير محدودة", "دعم مستمر"],
  },
];

function useInView(threshold = 0.1) {
  const ref = useRef<HTMLDivElement>(null);
  const [inView, setInView] = useState(false);
  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    const observer = new IntersectionObserver(([e]) => { if (e.isIntersecting) { setInView(true); observer.disconnect(); } }, { threshold });
    observer.observe(el);
    return () => observer.disconnect();
  }, [threshold]);
  return { ref, inView };
}

export default function PortfolioPage() {
  const [menuOpen, setMenuOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);

  // Pricing calculator
  const [service, setService] = useState<keyof typeof PRICING>("reels");
  const [durationIdx, setDurationIdx] = useState(0);
  const [qty, setQty] = useState(1);

  // Contact form
  const [form, setForm] = useState({ name: "", phone: "", service: "", message: "" });
  const [submitted, setSubmitted] = useState(false);

  const hero = useInView(0.05);
  const comparison = useInView(0.1);
  const calculator = useInView(0.1);
  const pkgSection = useInView(0.1);
  const portfolio = useInView(0.05);
  const contact = useInView(0.1);

  useEffect(() => {
    const fn = () => setScrolled(window.scrollY > 40);
    window.addEventListener("scroll", fn);
    return () => window.removeEventListener("scroll", fn);
  }, []);

  const scrollTo = (id: string) => { setMenuOpen(false); document.getElementById(id)?.scrollIntoView({ behavior: "smooth" }); };

  const currentPricing = PRICING[service];
  const duration = currentPricing.durations[Math.min(durationIdx, currentPricing.durations.length - 1)];
  const totalPrice = Math.round(currentPricing.base * duration.mult * qty);

  const handleServiceChange = (s: keyof typeof PRICING) => {
    setService(s);
    setDurationIdx(0);
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const msg = `مرحباً، أنا ${form.name}%0Aرقم التواصل: ${form.phone}%0Aالخدمة المطلوبة: ${form.service}%0Aالرسالة: ${form.message}`;
    window.open(`https://wa.me/966572929651?text=${msg}`, "_blank");
    setSubmitted(true);
  };

  const navLinks = [
    { label: "الرئيسية", id: "hero" },
    { label: "أعمالي", id: "portfolio" },
    { label: "الأسعار", id: "calculator" },
    { label: "الباقات", id: "packages" },
    { label: "تواصل", id: "contact" },
  ];

  return (
    <div className="min-h-screen bg-background text-foreground font-sans" dir="rtl">

      {/* ── NAV ── */}
      <nav className={`fixed top-0 inset-x-0 z-50 transition-all duration-300 ${scrolled ? "bg-background/95 backdrop-blur-md border-b border-border shadow-lg shadow-black/20" : "bg-transparent"}`}>
        <div className="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
          <span className="text-xl font-black gold-gradient cursor-pointer" onClick={() => scrollTo("hero")}>Osama</span>
          <div className="hidden md:flex items-center gap-7">
            {navLinks.map(n => (
              <button key={n.id} onClick={() => scrollTo(n.id)} className="nav-link text-muted-foreground hover:text-foreground transition-colors text-sm font-medium" data-testid={`nav-${n.id}`}>{n.label}</button>
            ))}
          </div>
          <button className="hidden md:block gold-gradient-bg text-background font-bold px-5 py-2 rounded-lg text-sm hover:opacity-90 transition-opacity" onClick={() => scrollTo("contact")} data-testid="nav-cta">ابدأ مشروعك</button>
          <button className="md:hidden flex flex-col gap-1.5 p-2" onClick={() => setMenuOpen(v => !v)} data-testid="nav-toggle">
            <span className={`block w-6 h-0.5 bg-foreground transition-all ${menuOpen ? "rotate-45 translate-y-2" : ""}`} />
            <span className={`block w-6 h-0.5 bg-foreground transition-all ${menuOpen ? "opacity-0" : ""}`} />
            <span className={`block w-6 h-0.5 bg-foreground transition-all ${menuOpen ? "-rotate-45 -translate-y-2" : ""}`} />
          </button>
        </div>
        {menuOpen && (
          <div className="md:hidden bg-card border-b border-border px-6 py-4 flex flex-col gap-4">
            {navLinks.map(n => (
              <button key={n.id} onClick={() => scrollTo(n.id)} className="text-right text-muted-foreground hover:text-primary transition-colors font-medium py-1" data-testid={`mobile-nav-${n.id}`}>{n.label}</button>
            ))}
          </div>
        )}
      </nav>

      {/* ── HERO ── */}
      <section id="hero" className="hero-bg min-h-screen flex items-center justify-center relative overflow-hidden pt-20">
        <div className="absolute top-1/4 left-1/4 w-80 h-80 rounded-full bg-primary/6 blur-3xl pointer-events-none" />
        <div className="absolute bottom-1/3 right-1/4 w-96 h-96 rounded-full bg-primary/4 blur-3xl pointer-events-none" />

        <div ref={hero.ref} className={`text-center px-6 max-w-4xl mx-auto transition-all duration-700 ${hero.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
          <div className="inline-flex items-center gap-2 bg-primary/10 border border-primary/20 rounded-full px-5 py-2 mb-8">
            <span className="w-2 h-2 rounded-full bg-primary animate-pulse" />
            <span className="text-primary text-sm font-semibold">مونتير محترف · المملكة العربية السعودية</span>
          </div>
          <h1 className="text-4xl sm:text-5xl md:text-7xl font-black mb-6 leading-tight">
            <span className="text-foreground">أصنع محتوى</span><br />
            <span className="gold-gradient">يوقف التمرير</span>
          </h1>
          <p className="text-muted-foreground text-lg md:text-xl leading-relaxed mb-10 max-w-2xl mx-auto">
            مونتير احترافي متخصص في ريلز الجذّابة، الإعلانات التجارية، والفيديوهات المؤسسية — أحوّل أفكارك إلى محتوى بصري يُعبّر عن علامتك بأسلوب فريد.
          </p>
          <div className="flex flex-wrap gap-4 justify-center mb-14">
            <button onClick={() => scrollTo("portfolio")} className="gold-gradient-bg text-background font-bold px-8 py-3.5 rounded-xl hover:opacity-90 transition-all hover:scale-105 shadow-lg shadow-primary/30" data-testid="hero-portfolio">شاهد أعمالي</button>
            <button onClick={() => scrollTo("calculator")} className="border border-primary/40 text-primary font-semibold px-8 py-3.5 rounded-xl hover:bg-primary/10 transition-all" data-testid="hero-price">احسب سعرك</button>
          </div>
          {/* Stats */}
          <div className="grid grid-cols-3 gap-6 max-w-md mx-auto">
            {[["50+", "مشروع منجز"], ["100%", "رضا العملاء"], ["48h", "متوسط التسليم"]].map(([num, label]) => (
              <div key={label} className="glass-card rounded-xl p-4 border border-border/60">
                <p className="text-2xl font-black gold-gradient">{num}</p>
                <p className="text-xs text-muted-foreground mt-1">{label}</p>
              </div>
            ))}
          </div>
        </div>

        <div className="absolute bottom-10 left-1/2 -translate-x-1/2 flex flex-col items-center gap-2 opacity-40">
          <span className="text-xs text-muted-foreground">مرر للأسفل</span>
          <div className="w-0.5 h-10 bg-gradient-to-b from-primary to-transparent" />
        </div>
      </section>

      {/* ── BEFORE / AFTER ── */}
      <section id="comparison" className="py-24 px-6 bg-card/30">
        <div className="max-w-4xl mx-auto">
          <div ref={comparison.ref} className={`text-center mb-12 transition-all duration-700 ${comparison.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            <p className="text-primary text-sm font-semibold uppercase tracking-widest mb-3">الفرق الحقيقي</p>
            <h2 className="text-3xl md:text-4xl font-black mb-4">قبل وبعد المونتاج</h2>
            <div className="section-divider mb-4" />
            <p className="text-muted-foreground max-w-xl mx-auto">اسحب الشريط لترى الفرق بين اللقطات الخام والمونتاج الاحترافي</p>
          </div>

          <div className={`transition-all duration-700 delay-200 ${comparison.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"} max-w-sm mx-auto`}>
            <BeforeAfterSlider
              beforeLabel="قبل المونتاج"
              afterLabel="بعد المونتاج"
              beforeContent={
                <video
                  src="/before-video.mov"
                  autoPlay
                  loop
                  muted
                  playsInline
                  className="w-full h-full object-cover"
                  style={{ aspectRatio: "9/16", width: "100%", display: "block" }}
                />
              }
              afterContent={
                <video
                  src="/after-video.mov"
                  autoPlay
                  loop
                  muted
                  playsInline
                  className="w-full h-full object-cover"
                  style={{ aspectRatio: "9/16", width: "100%", display: "block" }}
                />
              }
            />
            <p className="text-center text-muted-foreground text-sm mt-4">اسحب الشريط يميناً ويساراً</p>
          </div>
        </div>
      </section>

      {/* ── PRICING CALCULATOR ── */}
      <section id="calculator" className="py-24 px-6">
        <div className="max-w-3xl mx-auto">
          <div ref={calculator.ref} className={`text-center mb-12 transition-all duration-700 ${calculator.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            <p className="text-primary text-sm font-semibold uppercase tracking-widest mb-3">الأسعار</p>
            <h2 className="text-3xl md:text-4xl font-black mb-4">احسب سعر مشروعك</h2>
            <div className="section-divider mb-4" />
            <p className="text-muted-foreground max-w-xl mx-auto">اختر نوع الخدمة والمدة وعدد القطع لتحصل على السعر فوراً</p>
          </div>

          <div className={`glass-card border border-border/60 rounded-2xl p-6 md:p-8 transition-all duration-700 delay-200 ${calculator.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            {/* Service select */}
            <div className="mb-6">
              <label className="block text-sm font-semibold text-foreground mb-3">نوع الخدمة</label>
              <div className="grid grid-cols-3 gap-3">
                {(Object.keys(PRICING) as (keyof typeof PRICING)[]).map(key => (
                  <button
                    key={key}
                    onClick={() => handleServiceChange(key)}
                    className={`py-3 px-4 rounded-xl text-sm font-semibold border transition-all ${service === key ? "gold-gradient-bg text-background border-primary shadow-lg shadow-primary/20" : "bg-card border-border text-muted-foreground hover:border-primary/40 hover:text-foreground"}`}
                    data-testid={`calc-service-${key}`}
                  >
                    {PRICING[key].label}
                  </button>
                ))}
              </div>
            </div>

            {/* Duration */}
            <div className="mb-6">
              <label className="block text-sm font-semibold text-foreground mb-3">مدة الفيديو</label>
              <div className="flex flex-wrap gap-3">
                {currentPricing.durations.map((d, i) => (
                  <button
                    key={d.label}
                    onClick={() => setDurationIdx(i)}
                    className={`py-2 px-5 rounded-xl text-sm font-medium border transition-all ${durationIdx === i ? "gold-gradient-bg text-background border-primary shadow-md shadow-primary/20" : "bg-card border-border text-muted-foreground hover:border-primary/40 hover:text-foreground"}`}
                    data-testid={`calc-duration-${i}`}
                  >
                    {d.label}
                  </button>
                ))}
              </div>
            </div>

            {/* Quantity */}
            <div className="mb-8">
              <label className="block text-sm font-semibold text-foreground mb-3">عدد القطع</label>
              <div className="flex items-center gap-4">
                <button onClick={() => setQty(q => Math.max(1, q - 1))} className="w-10 h-10 rounded-xl bg-card border border-border hover:border-primary/40 flex items-center justify-center text-lg font-bold transition-colors" data-testid="calc-qty-minus">−</button>
                <span className="text-2xl font-black w-12 text-center text-foreground" data-testid="calc-qty">{qty}</span>
                <button onClick={() => setQty(q => Math.min(20, q + 1))} className="w-10 h-10 rounded-xl bg-card border border-border hover:border-primary/40 flex items-center justify-center text-lg font-bold transition-colors" data-testid="calc-qty-plus">+</button>
              </div>
            </div>

            {/* Result */}
            <div className="bg-gradient-to-r from-primary/10 to-primary/5 border border-primary/20 rounded-2xl p-6 flex flex-col sm:flex-row items-center justify-between gap-4">
              <div>
                <p className="text-muted-foreground text-sm mb-1">السعر التقديري</p>
                <p className="text-xs text-muted-foreground">{currentPricing.label} · {duration.label} · {qty} {qty === 1 ? "قطعة" : "قطع"}</p>
              </div>
              <div className="text-center sm:text-left">
                <p className="text-4xl font-black gold-gradient" data-testid="calc-total">{totalPrice.toLocaleString()}</p>
                <p className="text-primary text-sm font-semibold">ريال سعودي</p>
              </div>
            </div>

            <p className="text-center text-muted-foreground/60 text-xs mt-4">* الأسعار تقديرية، يتم التأكيد بعد مناقشة التفاصيل</p>

            <button onClick={() => scrollTo("contact")} className="w-full mt-4 gold-gradient-bg text-background font-bold py-3.5 rounded-xl hover:opacity-90 transition-opacity" data-testid="calc-cta">
              احجز الآن بهذا السعر
            </button>
          </div>
        </div>
      </section>

      {/* ── PACKAGES ── */}
      <section id="packages" className="py-24 px-6 bg-card/30">
        <div className="max-w-5xl mx-auto">
          <div ref={pkgSection.ref} className={`text-center mb-14 transition-all duration-700 ${pkgSection.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            <p className="text-primary text-sm font-semibold uppercase tracking-widest mb-3">الباقات</p>
            <h2 className="text-3xl md:text-4xl font-black mb-4">اختر الباقة المناسبة</h2>
            <div className="section-divider mb-4" />
            <p className="text-muted-foreground max-w-xl mx-auto">باقات شهرية مصممة لتناسب احتياجاتك وميزانيتك</p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {packages.map((pkg, i) => (
              <div
                key={pkg.id}
                className={`relative rounded-2xl border p-7 flex flex-col bg-gradient-to-b ${pkg.color} ${pkg.border} transition-all duration-700 hover:-translate-y-2 hover:shadow-xl ${pkgSection.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}
                style={{ transitionDelay: pkgSection.inView ? `${i * 120}ms` : "0ms" }}
                data-testid={pkg.id}
              >
                {pkg.highlight && (
                  <div className="absolute -top-3 left-1/2 -translate-x-1/2 gold-gradient-bg text-background text-xs font-bold px-4 py-1 rounded-full shadow-lg">
                    الأكثر طلباً
                  </div>
                )}
                <h3 className="text-xl font-black text-foreground mb-2">{pkg.name}</h3>
                <div className="flex items-end gap-1 mb-6">
                  <span className="text-4xl font-black gold-gradient">{pkg.price}</span>
                  <span className="text-muted-foreground text-sm mb-1">ر.س / {pkg.period}</span>
                </div>
                <ul className="flex-1 space-y-3 mb-8">
                  {pkg.features.map(f => (
                    <li key={f} className="flex items-center gap-3 text-sm text-muted-foreground">
                      <span className="w-5 h-5 rounded-full gold-gradient-bg flex-shrink-0 flex items-center justify-center">
                        <svg viewBox="0 0 16 16" fill="currentColor" className="w-3 h-3 text-background">
                          <path fillRule="evenodd" d="M12.416 3.376a.75.75 0 0 1 .208 1.04l-5 7.5a.75.75 0 0 1-1.154.114l-3-3a.75.75 0 0 1 1.06-1.06l2.353 2.353 4.493-6.74a.75.75 0 0 1 1.04-.207Z" clipRule="evenodd" />
                        </svg>
                      </span>
                      {f}
                    </li>
                  ))}
                </ul>
                <button
                  onClick={() => scrollTo("contact")}
                  className={`w-full py-3 rounded-xl font-bold text-sm transition-all hover:scale-105 ${pkg.highlight ? "gold-gradient-bg text-background shadow-lg shadow-primary/20" : "border border-current text-primary hover:bg-primary/10"}`}
                  data-testid={`${pkg.id}-cta`}
                >
                  اشترك الآن
                </button>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* ── PORTFOLIO ── */}
      <section id="portfolio" className="py-24 px-6">
        <div className="max-w-6xl mx-auto">
          <div ref={portfolio.ref} className={`text-center mb-14 transition-all duration-700 ${portfolio.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            <p className="text-primary text-sm font-semibold uppercase tracking-widest mb-3">معرض الأعمال</p>
            <h2 className="text-3xl md:text-4xl font-black mb-4">أحدث أعمالي</h2>
            <div className="section-divider mb-4" />
            <p className="text-muted-foreground max-w-xl mx-auto">نماذج حقيقية من مشاريعي — كل فيديو يعكس دقة المونتاج واهتمامي بالتفاصيل</p>
          </div>

          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            {videos.map((video, i) => (
              <div
                key={video.id}
                className={`video-card glass-card rounded-2xl overflow-hidden border border-border/60 transition-all duration-700 ${portfolio.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}
                style={{ transitionDelay: portfolio.inView ? `${i * 100}ms` : "0ms" }}
                data-testid={`video-card-${video.id}`}
              >
                <div className="relative bg-muted overflow-hidden" style={{ paddingBottom: "177.77%" }}>
                  <iframe className="absolute inset-0 w-full h-full" src={video.url} title={video.title} allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen scrolling="no" frameBorder="0" />
                </div>
                <div className="p-4 flex items-center justify-between">
                  <div>
                    <span className="text-xs text-primary font-semibold bg-primary/10 px-3 py-1 rounded-full">{video.category}</span>
                    <h3 className="text-foreground font-semibold mt-2 text-sm">{video.title}</h3>
                  </div>
                  <a href={video.link} target="_blank" rel="noopener noreferrer" className="flex-shrink-0 w-9 h-9 rounded-lg bg-primary/10 hover:bg-primary/20 flex items-center justify-center transition-colors mr-3" data-testid={`video-link-${video.id}`}>
                    <svg viewBox="0 0 24 24" fill="currentColor" className="w-4 h-4 text-primary">
                      <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z" />
                    </svg>
                  </a>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* ── CONTACT ── */}
      <section id="contact" className="py-24 px-6 bg-card/30">
        <div className="max-w-4xl mx-auto">
          <div ref={contact.ref} className={`text-center mb-14 transition-all duration-700 ${contact.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            <p className="text-primary text-sm font-semibold uppercase tracking-widest mb-3">تواصل معي</p>
            <h2 className="text-3xl md:text-4xl font-black mb-4">هل لديك مشروع؟</h2>
            <div className="section-divider mb-4" />
            <p className="text-muted-foreground max-w-lg mx-auto">أرسل تفاصيل مشروعك وسأرد عليك خلال 24 ساعة</p>
          </div>

          <div className={`grid grid-cols-1 lg:grid-cols-5 gap-8 transition-all duration-700 delay-200 ${contact.inView ? "opacity-100 translate-y-0" : "opacity-0 translate-y-8"}`}>
            {/* Form */}
            <div className="lg:col-span-3 glass-card border border-border/60 rounded-2xl p-6 md:p-8">
              {submitted ? (
                <div className="text-center py-10">
                  <div className="w-16 h-16 rounded-full gold-gradient-bg flex items-center justify-center mx-auto mb-4 shadow-lg shadow-primary/30">
                    <svg viewBox="0 0 24 24" fill="currentColor" className="w-8 h-8 text-background">
                      <path fillRule="evenodd" d="M2.25 12c0-5.385 4.365-9.75 9.75-9.75s9.75 4.365 9.75 9.75-4.365 9.75-9.75 9.75S2.25 17.385 2.25 12Zm13.36-1.814a.75.75 0 1 0-1.22-.872l-3.236 4.53L9.53 12.22a.75.75 0 0 0-1.06 1.06l2.25 2.25a.75.75 0 0 0 1.14-.094l3.75-5.25Z" clipRule="evenodd" />
                    </svg>
                  </div>
                  <h3 className="text-xl font-bold text-foreground mb-2">تم الإرسال!</h3>
                  <p className="text-muted-foreground">تم فتح واتساب لإكمال التواصل. سأرد عليك قريباً.</p>
                  <button onClick={() => setSubmitted(false)} className="mt-6 text-primary text-sm underline">إرسال رسالة أخرى</button>
                </div>
              ) : (
                <form onSubmit={handleSubmit} className="space-y-5" data-testid="contact-form">
                  <h3 className="text-lg font-bold text-foreground mb-6">أرسل طلبك</h3>
                  <div className="grid grid-cols-1 sm:grid-cols-2 gap-5">
                    <div>
                      <label className="block text-sm font-medium text-muted-foreground mb-2">الاسم *</label>
                      <input required value={form.name} onChange={e => setForm(f => ({ ...f, name: e.target.value }))} className="w-full bg-background border border-border rounded-xl px-4 py-3 text-foreground text-sm focus:outline-none focus:border-primary transition-colors placeholder:text-muted-foreground/50" placeholder="اسمك الكريم" data-testid="input-name" />
                    </div>
                    <div>
                      <label className="block text-sm font-medium text-muted-foreground mb-2">رقم الهاتف *</label>
                      <input required value={form.phone} onChange={e => setForm(f => ({ ...f, phone: e.target.value }))} className="w-full bg-background border border-border rounded-xl px-4 py-3 text-foreground text-sm focus:outline-none focus:border-primary transition-colors placeholder:text-muted-foreground/50" placeholder="+966 5XX XXX XXX" dir="ltr" data-testid="input-phone" />
                    </div>
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-muted-foreground mb-2">الخدمة المطلوبة</label>
                    <select value={form.service} onChange={e => setForm(f => ({ ...f, service: e.target.value }))} className="w-full bg-background border border-border rounded-xl px-4 py-3 text-foreground text-sm focus:outline-none focus:border-primary transition-colors" data-testid="input-service">
                      <option value="">اختر الخدمة</option>
                      <option value="مونتاج ريلز">مونتاج ريلز</option>
                      <option value="فيديو إعلاني">فيديو إعلاني</option>
                      <option value="فيديو مؤسسي">فيديو مؤسسي</option>
                      <option value="باقة شهرية">باقة شهرية</option>
                    </select>
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-muted-foreground mb-2">تفاصيل المشروع</label>
                    <textarea value={form.message} onChange={e => setForm(f => ({ ...f, message: e.target.value }))} rows={4} className="w-full bg-background border border-border rounded-xl px-4 py-3 text-foreground text-sm focus:outline-none focus:border-primary transition-colors placeholder:text-muted-foreground/50 resize-none" placeholder="اشرح مشروعك بإيجاز..." data-testid="input-message" />
                  </div>
                  <button type="submit" className="w-full gold-gradient-bg text-background font-bold py-3.5 rounded-xl hover:opacity-90 transition-all hover:scale-[1.02] shadow-lg shadow-primary/20 flex items-center justify-center gap-2" data-testid="form-submit">
                    <svg viewBox="0 0 24 24" fill="currentColor" className="w-5 h-5">
                      <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893A11.821 11.821 0 0020.885 3.488" />
                    </svg>
                    إرسال عبر واتساب
                  </button>
                </form>
              )}
            </div>

            {/* Contact info sidebar */}
            <div className="lg:col-span-2 flex flex-col gap-5">
              <a href="https://wa.me/966572929651" target="_blank" rel="noopener noreferrer" className="contact-link glass-card border border-border/60 hover:border-green-500/40 rounded-2xl p-5 flex items-center gap-4 group hover:bg-green-500/5 transition-all" data-testid="contact-whatsapp">
                <div className="w-12 h-12 rounded-xl bg-green-500/10 border border-green-500/20 flex items-center justify-center flex-shrink-0 group-hover:bg-green-500/20 transition-colors">
                  <svg viewBox="0 0 24 24" fill="currentColor" className="w-6 h-6 text-green-400">
                    <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893A11.821 11.821 0 0020.885 3.488" />
                  </svg>
                </div>
                <div>
                  <p className="text-xs text-muted-foreground mb-0.5">واتساب</p>
                  <p className="font-bold text-foreground" dir="ltr">+966 572 929 651</p>
                </div>
              </a>

              <a href="mailto:mjlyyy2007@gmail.com" className="contact-link glass-card border border-border/60 hover:border-primary/40 rounded-2xl p-5 flex items-center gap-4 group hover:bg-primary/5 transition-all" data-testid="contact-email">
                <div className="w-12 h-12 rounded-xl bg-primary/10 border border-primary/20 flex items-center justify-center flex-shrink-0 group-hover:bg-primary/20 transition-colors">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.5" className="w-6 h-6 text-primary">
                    <path strokeLinecap="round" strokeLinejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 0 1-2.25 2.25h-15a2.25 2.25 0 0 1-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0 0 19.5 4.5h-15a2.25 2.25 0 0 0-2.25 2.25m19.5 0v.243a2.25 2.25 0 0 1-1.07 1.916l-7.5 4.615a2.25 2.25 0 0 1-2.36 0L3.32 8.91a2.25 2.25 0 0 1-1.07-1.916V6.75" />
                  </svg>
                </div>
                <div>
                  <p className="text-xs text-muted-foreground mb-0.5">البريد الإلكتروني</p>
                  <p className="font-bold text-foreground text-sm break-all" dir="ltr">mjlyyy2007@gmail.com</p>
                </div>
              </a>

              <div className="glass-card border border-border/60 rounded-2xl p-5">
                <p className="text-sm font-bold text-foreground mb-4">ساعات العمل</p>
                <div className="space-y-2 text-sm">
                  <div className="flex justify-between"><span className="text-muted-foreground">الأحد – الخميس</span><span className="text-foreground font-medium">9ص – 10م</span></div>
                  <div className="flex justify-between"><span className="text-muted-foreground">الجمعة – السبت</span><span className="text-foreground font-medium">12م – 8م</span></div>
                </div>
              </div>

              <a href="https://wa.me/966572929651?text=مرحباً، أريد الاستفسار عن خدمات المونتاج" target="_blank" rel="noopener noreferrer" className="gold-gradient-bg text-background font-bold py-4 rounded-xl hover:opacity-90 transition-all hover:scale-105 shadow-lg shadow-primary/30 text-center block" data-testid="contact-cta">
                ابدأ مشروعك الآن
              </a>
            </div>
          </div>
        </div>
      </section>

      {/* ── FOOTER ── */}
      <footer className="border-t border-border py-8 px-6 text-center">
        <p className="text-muted-foreground text-sm">© {new Date().getFullYear()} Osama video editor — جميع الحقوق محفوظة</p>
        <p className="text-muted-foreground/50 text-xs mt-1">مونتير محترف | المملكة العربية السعودية</p>
      </footer>
    </div>
  );
}
