---
title: 音乐点播室 · 随时来点一首
description: 支持网易云官方外链 + 服务器音乐文件播放，欢迎点歌或听本站歌单～
pubDate: 2026-01-01  # 必填日期字段（根据你的 schema 调整为 publishedAt 或 pubDate）
tags: ["音乐","点播","网易云","服务器播放"]
author: 瑶子
draft: false
---

# 音乐点播室

<div class="grid">

  <!-- 卡片 1：网易云点歌 -->
  <div class="card gradient1">
    <h2>🎵 网易云点歌</h2>
    <p>粘贴网易云歌曲链接（公开歌曲即可播放）</p>
    <input type="text" id="song-url" placeholder="粘贴歌曲链接...">
    <button onclick="loadSong()">加载播放</button>
    <div id="status"></div>
  </div>

  <!-- 卡片 2：服务器歌单 -->
  <div class="card gradient2">
    <h2>🎶 本站歌单</h2>
    <p>点击直接播放服务器音乐文件</p>
    <div id="server-list"></div>
  </div>

  <!-- 卡片 3：播放器 -->
  <div class="card white">
    <h3>♪ 当前歌曲</h3>
    <p id="song-title">暂无歌曲</p>
    <iframe id="netease-player" style="display:none;"></iframe>
    <audio id="server-player" controls></audio>
  </div>

</div>

<style>
  .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 24px; margin: 32px 0; }
  .card { border-radius: 16px; padding: 28px; box-shadow: 0 10px 30px rgba(0,0,0,0.15); }
  .gradient1 { background: linear-gradient(135deg, #667eea, #764ba2); color: white; }
  .gradient2 { background: linear-gradient(135deg, #f093fb, #f5576c); color: white; }
  .white { background: white; }
  input, button { width: 100%; padding: 14px; margin: 16px 0 0; border-radius: 12px; border: none; font-size: 1em; }
  button { background: rgba(255,255,255,0.25); color: white; cursor: pointer; font-weight: bold; }
  #server-list > div { padding: 12px; background: rgba(255,255,255,0.2); border-radius: 8px; margin: 8px 0; cursor: pointer; }
  #server-list > div:hover { background: rgba(255,255,255,0.4); }
</style>

<script is:inline>
  // 配置服务器音乐（文件放 public/music/）
  const SERVER_MUSIC = [
    { file: 'song1.mp3', title: '稻香 - 周杰伦' },
    { file: 'song2.mp3', title: '晴天 - 周杰伦' },
    // 添加更多
  ];

  const list = document.getElementById('server-list');
  SERVER_MUSIC.forEach(s => {
    const div = document.createElement('div');
    div.textContent = s.title;
    div.onclick = () => {
      const p = document.getElementById('server-player');
      const n = document.getElementById('netease-player');
      const t = document.getElementById('song-title');
      p.src = `/music/${s.file}`;
      p.style.display = 'block';
      n.style.display = 'none';
      t.textContent = s.title + '（服务器文件）';
      p.play();
    };
    list.appendChild(div);
  });

  function loadSong() {
    const input = document.getElementById('song-url').value.trim();
    const status = document.getElementById('status');
    const title = document.getElementById('song-title');
    const p = document.getElementById('netease-player');
    const sp = document.getElementById('server-player');

    const match = input.match(/id=(\d+)/);
    if (!match) {
      status.textContent = '链接格式错误';
      status.style.color = '#ef4444';
      return;
    }

    const id = match[1];
    status.textContent = '加载中...';
    p.src = `https://music.163.com/outchain/player?type=2&id=${id}&auto=1&height=66`;
    p.style.display = 'block';
    sp.style.display = 'none';
    title.textContent = '网易云歌曲播放中';
    status.textContent = '加载成功';
    status.style.color = '#22c55e';
  }
</script>