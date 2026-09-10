<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Salh Alheib Klel Studio - AI Web DAW & Video Creator</title>
  <!-- Tailwind CSS & React & Babel CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <style>
    body { background-color: #0a0a0a; color: #f5f5f5; font-family: system-ui, -apple-system, sans-serif; }
    .grid-cols-16 { grid-template-columns: repeat(16, minmax(0, 1fr)); }
  </style>
</head>
<body class="select-none min-h-screen">
  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect, useRef } = React;

    function WebStudioApp() {
      const [isPlaying, setIsPlaying] = useState(false);
      const [bpm, setBpm] = useState(120);
      const [masterVolume, setMasterVolume] = useState(0.8);
      const [activeTab, setActiveTab] = useState('ai_studio');
      const [prompt, setPrompt] = useState('إيقاع لو-فاي هادئ مع بيانو شرقي وناي');
      const [steps, setSteps] = useState([true, false, false, false, true, false, false, false, true, false, false, false, true, false, false, false]);

      // Video Studio State
      const [videoPrompt, setVideoPrompt] = useState('مشهد استرخاء هادئ للسماء والنجوم في ليلة دافئة مع الأمطار المتساقطة على زجاج النافذة سينمائي جودة عالية');
      const [videoCategory, setVideoCategory] = useState('sleep');
      const [videoAspectRatio, setVideoAspectRatio] = useState('9:16');
      const [videoDuration, setVideoDuration] = useState(6);
      const [generatingVideo, setGeneratingVideo] = useState(false);
      const [generatedVideos, setGeneratedVideos] = useState([]);

      const [showPaymentModal, setShowPaymentModal] = useState(false);
      const [showHTMLModal, setShowHTMLModal] = useState(false);
      const [plan, setPlan] = useState('yearly');
      const [generatedSongs, setGeneratedSongs] = useState([]);
      const [loading, setLoading] = useState(false);
      const [imagePrompt, setImagePrompt] = useState('غلاف ألبوم موسيقي شرقي حديث مع أضواء نيون ورسومات سينمائية');
      const [generatedImages, setGeneratedImages] = useState([]);

      const handleGenerateSong = () => {
        if (!prompt.trim()) return;
        setLoading(true);
        setTimeout(() => {
          setGeneratedSongs(prev => [
            { id: Date.now(), title: prompt, audioUrl: '#', coverImageUrl: generatedImages[0] || '' },
            ...prev
          ]);
          setLoading(false);
        }, 1500);
      };

      const handleGenerateVideo = () => {
        if (!videoPrompt.trim()) return;
        setGeneratingVideo(true);
        setTimeout(() => {
          setGeneratedVideos(prev => [
            {
              id: 'vid_' + Date.now(),
              url: 'https://assets.mixkit.co/videos/preview/mixkit-stars-in-the-night-sky-background-4214-large.mp4',
              prompt: videoPrompt,
              category: videoCategory
            },
            ...prev
          ]);
          setGeneratingVideo(false);
        }, 2000);
      };

      return (
        <div className="min-h-screen w-full bg-neutral-950 text-neutral-100 flex flex-col font-sans">
          {/* Header Bar */}
          <header className="bg-neutral-900 border-b border-neutral-800 p-4 flex flex-wrap items-center justify-between gap-4 shadow-md">
            <div className="flex items-center gap-3">
              <h1 className="text-base font-black text-orange-400">Salh Alheib Klel Studio</h1>
              <div className="flex items-center gap-2 bg-neutral-950 px-3 py-1 rounded-xl border border-neutral-800">
                <span className="text-xs text-neutral-400 font-mono">BPM: {bpm}</span>
              </div>
            </div>

            <div className="flex items-center gap-2">
              <button 
                onClick={() => setIsPlaying(!isPlaying)}
                className={`px-4 py-2 rounded-xl font-bold text-xs transition ${isPlaying ? 'bg-orange-500 text-neutral-950 shadow-lg' : 'bg-neutral-800 text-neutral-200 hover:bg-neutral-700'}`}
              >
                {isPlaying ? 'إيقاف مؤقت' : 'تشغيل الإيقاع'}
              </button>
              <button 
                onClick={() => setShowPaymentModal(true)}
                className="px-4 py-2 bg-gradient-to-r from-amber-500 to-orange-500 text-neutral-950 font-extrabold rounded-xl text-xs shadow-md"
              >
                اشتري الباقة
              </button>
            </div>
          </header>

          {/* Navigation Tabs */}
          <nav className="flex justify-center gap-2 p-3 bg-neutral-900/50 border-b border-neutral-800 text-xs font-bold">
            <button onClick={() => setActiveTab('ai_studio')} className={`px-4 py-2 rounded-lg transition ${activeTab === 'ai_studio' ? 'bg-orange-500 text-neutral-950' : 'text-neutral-400 hover:bg-neutral-800'}`}>استوديو الذكاء الاصطناعي</button>
            <button onClick={() => setActiveTab('video_studio')} className={`px-4 py-2 rounded-lg transition ${activeTab === 'video_studio' ? 'bg-orange-500 text-neutral-950' : 'text-neutral-400 hover:bg-neutral-800'}`}>صانع الفيديوهات الذكي</button>
            <button onClick={() => setActiveTab('sequencer')} className={`px-4 py-2 rounded-lg transition ${activeTab === 'sequencer' ? 'bg-orange-500 text-neutral-950' : 'text-neutral-400 hover:bg-neutral-800'}`}>موزع الإيقاعات (Sequencer)</button>
          </nav>

          {/* Main Content Area */}
          <main className="flex-1 p-6 max-w-6xl w-full mx-auto flex flex-col gap-6">
            {activeTab === 'ai_studio' && (
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div className="bg-neutral-900 border border-neutral-800 rounded-2xl p-6 shadow-2xl flex flex-col gap-4">
                  <h2 className="font-extrabold text-sm text-orange-400 uppercase tracking-wider">توليد الموسيقى بالذكاء الاصطناعي</h2>
                  <div className="flex flex-col gap-2">
                    <label className="text-xs font-bold text-neutral-300">وصف النمط الموسيقي (Sound Prompt)</label>
                    <textarea
                      value={prompt}
                      onChange={(e) => setPrompt(e.target.value)}
                      rows={3}
                      className="w-full bg-neutral-950 border border-neutral-800 rounded-xl p-3 text-xs text-neutral-200 focus:outline-none focus:border-orange-500"
                    />
                  </div>
                  <button
                    onClick={handleGenerateSong}
                    disabled={loading}
                    className="w-full py-3 bg-gradient-to-r from-orange-500 to-amber-500 text-neutral-950 font-extrabold text-sm rounded-xl shadow-lg transition hover:brightness-110 disabled:opacity-50"
                  >
                    {loading ? 'جاري التوليد...' : 'توليد الأغنية والموسيقى الآن'}
                  </button>
                </div>

                <div className="bg-neutral-900 border border-neutral-800 rounded-2xl p-6 shadow-2xl flex flex-col gap-4">
                  <h2 className="text-xs font-bold text-orange-400 uppercase tracking-wider">الأغاني المولدة في الموقع ({generatedSongs.length})</h2>
                  {generatedSongs.length === 0 ? (
                    <div className="flex flex-col items-center justify-center h-48 text-neutral-500 text-xs border border-dashed border-neutral-800 rounded-xl">
                      <span>لا توجد أغانٍ مولدة بعد. اضغط على "توليد الأغنية" للبدء!</span>
                    </div>
                  ) : (
                    <div className="flex flex-col gap-3">
                      {generatedSongs.map(song => (
                        <div key={song.id} className="p-3 bg-neutral-950 border border-neutral-800 rounded-xl flex items-center justify-between">
                          <span className="text-xs font-bold text-neutral-200">{song.title}</span>
                          <span className="text-[10px] text-amber-400 font-mono">Salh Alheib Klel Studio</span>
                        </div>
                      ))}
                    </div>
                  )}
                </div>
              </div>
            )}

            {activeTab === 'video_studio' && (
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div className="bg-neutral-900 border border-neutral-800 rounded-2xl p-6 shadow-2xl flex flex-col gap-4">
                  <h2 className="font-extrabold text-sm text-orange-400 uppercase tracking-wider">صانع ومحرر الفيديوهات الذكي</h2>
                  <div className="flex flex-col gap-2">
                    <label className="text-xs font-bold text-neutral-300">وصف مشهد الفيديو بالكامل</label>
                    <textarea
                      value={videoPrompt}
                      onChange={(e) => setVideoPrompt(e.target.value)}
                      rows={3}
                      className="w-full bg-neutral-950 border border-neutral-800 rounded-xl p-3 text-xs text-neutral-200 focus:outline-none focus:border-orange-500"
                    />
                  </div>
                  <button
                    onClick={handleGenerateVideo}
                    disabled={generatingVideo}
                    className="w-full py-3 bg-gradient-to-r from-orange-500 to-amber-500 text-neutral-950 font-extrabold text-sm rounded-xl shadow-lg transition hover:brightness-110 disabled:opacity-50"
                  >
                    {generatingVideo ? 'جاري تصميم الفيديو...' : 'توليد وإنشاء الفيديو الآن'}
                  </button>
                </div>

                <div className="bg-neutral-900 border border-neutral-800 rounded-2xl p-6 shadow-2xl flex flex-col gap-4">
                  <h2 className="text-xs font-bold text-orange-400 uppercase tracking-wider">الفيديوهات المولدة ({generatedVideos.length})</h2>
                  {generatedVideos.length === 0 ? (
                    <div className="flex flex-col items-center justify-center h-48 text-neutral-500 text-xs border border-dashed border-neutral-800 rounded-xl">
                      <span>لا توجد فيديوهات مولدة بعد. اضغط على زر التوليد للبدء!</span>
                    </div>
                  ) : (
                    <div className="flex flex-col gap-3 max-h-64 overflow-y-auto">
                      {generatedVideos.map(vid => (
                        <div key={vid.id} className="p-3 bg-neutral-950 border border-neutral-800 rounded-xl flex flex-col gap-2">
                          <span className="text-xs font-bold text-neutral-200">{vid.prompt}</span>
                          <video src={vid.url} controls className="max-h-40 rounded-lg" />
                        </div>
                      ))}
                    </div>
                  )}
                </div>
              </div>
            )}

            {activeTab === 'sequencer' && (
              <div className="bg-neutral-900 p-6 rounded-2xl border border-neutral-800 flex flex-col gap-4">
                <h2 className="text-xs font-bold text-neutral-400 uppercase">Channel Rack (16 Steps)</h2>
                <div className="grid grid-cols-16 gap-1.5">
                  {steps.map((active, i) => (
                    <button
                      key={i}
                      onClick={() => {
                        const copy = [...steps];
                        copy[i] = !copy[i];
                        setSteps(copy);
                      }}
                      className={`h-12 rounded-lg font-bold text-xs transition ${active ? 'bg-orange-500 text-neutral-950' : 'bg-neutral-950 border border-neutral-800 text-neutral-500'}`}
                    >
                      {i + 1}
                    </button>
                  ))}
                </div>
              </div>
            )}
          </main>

          {/* Payment Modal */}
          {showPaymentModal && (
            <div className="fixed inset-0 bg-neutral-950/80 backdrop-blur-md z-50 flex items-center justify-center p-4">
              <div className="w-full max-w-md bg-neutral-900 border border-neutral-800 rounded-2xl p-6 shadow-2xl flex flex-col gap-5 relative">
                <button onClick={() => setShowPaymentModal(false)} className="absolute top-4 left-4 text-neutral-400 hover:text-neutral-100 text-xs font-bold">✕ إغلاق</button>
                <div className="text-center pt-2">
                  <h3 className="text-lg font-black text-neutral-100">باقات الاشتراكات الدائمة</h3>
                  <p className="text-xs text-neutral-400 mt-1">اشترك الآن للحصول على كافة مزايا الموقع والتوليد غير المحدود</p>
                </div>
                <div className="grid grid-cols-2 gap-3">
                  <div onClick={() => setPlan('monthly')} className={`p-4 rounded-xl border cursor-pointer text-center ${plan === 'monthly' ? 'bg-orange-500/10 border-orange-500 text-orange-300' : 'bg-neutral-950 border-neutral-800 text-neutral-400'}`}>
                    <span className="text-xs font-bold block">اشتراك شهري</span>
                    <span className="text-2xl font-black text-neutral-100 block mt-1">$20</span>
                  </div>
                  <div onClick={() => setPlan('yearly')} className={`p-4 rounded-xl border cursor-pointer text-center ${plan === 'yearly' ? 'bg-amber-500/10 border-amber-500 text-amber-300' : 'bg-neutral-950 border-neutral-800 text-neutral-400'}`}>
                    <span className="text-xs font-bold block">اشتراك سنوي</span>
                    <span className="text-2xl font-black text-neutral-100 block mt-1">$47</span>
                  </div>
                </div>
                <div className="bg-neutral-950 border border-neutral-800 rounded-xl p-3 text-center text-xs">
                  <span className="text-neutral-400">حساب الدفع عبر PayPal: </span>
                  <span className="text-amber-400 font-mono font-bold select-all">aalhee34@gmail.com</span>
                </div>
                <a href={`https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&business=aalhee34@gmail.com&item_name=${encodeURIComponent(plan === 'monthly' ? 'AI Music Studio Monthly Subscription ($20)' : 'AI Music Studio Yearly Subscription ($47)')}&amount=${plan === 'monthly' ? '20.00' : '47.00'}&currency_code=USD`} target="_blank" rel="noopener noreferrer" className="w-full py-3 bg-gradient-to-r from-amber-500 to-orange-500 text-neutral-950 font-extrabold text-xs rounded-xl text-center shadow-lg transition">
                  إتمام الدفع عبر PayPal ({plan === 'monthly' ? '$20' : '$47'})
                </a>
              </div>
            </div>
          )}
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root')).render(<WebStudioApp />);
  </script>
</body>
</html>
