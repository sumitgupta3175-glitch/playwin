import React, { useState } from 'react';
import { 
  Coins, Trophy, Gamepad2, CheckCircle2, LayoutDashboard, 
  Share2, ShieldCheck, Gift, Award, Sparkles, Clock, Users 
} from 'lucide-react';

// --- Main Application ---
export default function RewardPlay() {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [userState, setUserState] = useState({
    name: "Alex_ProGamer",
    coins: 4250,
    xp: 2450,
    level: 12,
    streak: 5,
    rank: 42
  });
  const [notification, setNotification] = useState(null);

  const notify = (msg) => {
    setNotification(msg);
    setTimeout(() => setNotification(null), 3000);
  };

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 font-sans p-4 md:p-8">
      {/* Toast Notification */}
      {notification && (
        <div className="fixed top-6 right-6 z-50 bg-purple-600 text-white px-6 py-3 rounded-xl shadow-2xl animate-bounce">
          {notification}
        </div>
      )}

      {/* Header */}
      <header className="flex justify-between items-center pb-8 max-w-6xl mx-auto">
        <h1 className="text-2xl font-black bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
          REWARDPLAY
        </h1>
        <div className="flex gap-4">
          <div className="flex items-center gap-2 bg-slate-900 px-4 py-2 rounded-full border border-slate-800">
            <Coins className="text-amber-400 w-5 h-5" />
            <span className="font-bold">{userState.coins}</span>
          </div>
        </div>
      </header>

      {/* Content Layout */}
      <div className="max-w-6xl mx-auto flex flex-col md:flex-row gap-8">
        <aside className="w-full md:w-64 flex flex-row md:flex-col gap-2 overflow-x-auto pb-4 md:pb-0">
          {['dashboard', 'tasks', 'games', 'withdraw'].map((tab) => (
            <button 
              key={tab} 
              onClick={() => setActiveTab(tab)}
              className={`px-4 py-3 rounded-xl capitalize font-bold text-sm transition-all ${activeTab === tab ? 'bg-purple-600 text-white' : 'bg-slate-900 text-slate-400'}`}
            >
              {tab}
            </button>
          ))}
        </aside>

        <main className="flex-1">
          {activeTab === 'dashboard' && <Dashboard user={userState} notify={notify} />}
          {activeTab === 'tasks' && <Tasks setCoins={setUserState} notify={notify} />}
          {activeTab === 'games' && <Games setCoins={setUserState} notify={notify} />}
          {activeTab === 'withdraw' && <Withdraw user={userState} setCoins={setUserState} notify={notify} />}
        </main>
      </div>
    </div>
  );
}

// --- Dashboard Component ---
function Dashboard({ user, notify }) {
  return (
    <div className="space-y-6">
      <div className="bg-gradient-to-r from-purple-900 to-slate-900 p-8 rounded-3xl border border-slate-800">
        <h2 className="text-3xl font-bold">Welcome back, {user.name}!</h2>
        <p className="text-slate-400 mt-2">Level {user.level} • {user.xp} XP</p>
      </div>
      <div className="grid grid-cols-2 gap-4">
        <div className="bg-slate-900 p-6 rounded-2xl border border-slate-800">
          <p className="text-sm text-slate-400">Daily Streak</p>
          <p className="text-2xl font-bold text-amber-400">{user.streak} Days</p>
        </div>
        <div className="bg-slate-900 p-6 rounded-2xl border border-slate-800">
          <p className="text-sm text-slate-400">Global Rank</p>
          <p className="text-2xl font-bold text-purple-400">#{user.rank}</p>
        </div>
      </div>
    </div>
  );
}

// --- Tasks Component ---
function Tasks({ setCoins, notify }) {
  const tasks = [{ id: 1, title: 'Follow Instagram', reward: 100 }, { id: 2, title: 'Watch Ad', reward: 50 }];
  const completeTask = (reward) => {
    setCoins(prev => ({ ...prev, coins: prev.coins + reward }));
    notify(`Earned ${reward} coins!`);
  };
  return (
    <div className="space-y-4">
      {tasks.map(t => (
        <div key={t.id} className="bg-slate-900 p-4 rounded-2xl flex justify-between items-center border border-slate-800">
          <span>{t.title}</span>
          <button onClick={() => completeTask(t.reward)} className="bg-emerald-600 px-4 py-2 rounded-lg text-sm font-bold">Complete</button>
        </div>
      ))}
    </div>
  );
}

// --- Games Component ---
function Games({ setCoins, notify }) {
  const play = () => {
    const win = Math.random() > 0.5 ? 100 : 0;
    if (win) {
      setCoins(prev => ({ ...prev, coins: prev.coins + win }));
      notify("Won 100 Coins!");
    } else {
      notify("Better luck next time!");
    }
  };
  return (
    <div className="grid grid-cols-1 gap-4">
      <div className="bg-slate-900 p-8 rounded-2xl border border-slate-800 text-center">
        <Gamepad2 className="w-12 h-12 text-purple-500 mx-auto mb-4" />
        <h3 className="text-xl font-bold mb-4">Spin The Wheel</h3>
        <button onClick={play} className="bg-purple-600 px-8 py-3 rounded-full font-bold">Spin Now</button>
      </div>
    </div>
  );
}

// --- Withdraw Component ---
function Withdraw({ user, setCoins, notify }) {
  const cashOut = () => {
    if (user.coins >= 2000) {
      setCoins(prev => ({ ...prev, coins: prev.coins - 2000 }));
      notify("Withdrawal Requested!");
    } else {
      notify("Need 2000 coins to withdraw!");
    }
  };
  return (
    <div className="bg-slate-900 p-8 rounded-2xl border border-slate-800">
      <h3 className="text-xl font-bold mb-4">Cash Out</h3>
      <p className="text-slate-400 mb-6">Min 2000 coins required.</p>
      <button onClick={cashOut} className="w-full bg-emerald-600 py-3 rounded-xl font-bold">Request $10</button>
    </div>
  );
}
