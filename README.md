<svg xmlns="http://www.w3.org/2000/svg" width="900" height="260" viewBox="0 0 900 260">
  <style>
    .bg{fill:#050505}
    .t{font:900 64px 'Courier New',monospace;text-anchor:middle}
    .main{fill:#fff}
    .r{fill:#ff0033;opacity:.8;animation:g1 2.5s infinite steps(1)}
    .c{fill:#00ffe1;opacity:.6;animation:g2 2.5s infinite steps(1)}
    .sub{font:700 18px 'Courier New',monospace;fill:#ff0033;text-anchor:middle;letter-spacing:4px}
    .term{font:16px 'Courier New',monospace;fill:#00ff41}
    .cur{animation:b 1s infinite steps(1)}
    .scan{fill:url(#s);opacity:.25}
    .bar{fill:#ff0033;animation:sw 4s infinite linear}
    @keyframes g1{0%,90%,100%{transform:none}92%{transform:translate(-6px,2px)}95%{transform:translate(4px,-3px)}}
    @keyframes g2{0%,90%,100%{transform:none}92%{transform:translate(6px,-2px)}96%{transform:translate(-4px,3px)}}
    @keyframes b{50%{opacity:0}}
    @keyframes sw{0%{transform:translateY(-10px)}100%{transform:translateY(270px)}}
  </style>
  <defs>
    <pattern id="s" width="4" height="4" patternUnits="userSpaceOnUse">
      <rect width="4" height="2" fill="#000"/>
    </pattern>
  </defs>
  <rect class="bg" width="900" height="260"/>
  <text class="term" x="30" y="40">root@fsociety:~# ./hello_friend.sh</text>
  <text class="t r" x="450" y="140">PEDRO HENRIQUE</text>
  <text class="t c" x="450" y="140">PEDRO HENRIQUE</text>
  <text class="t main" x="450" y="140">PEDRO HENRIQUE</text>
  <text class="sub" x="450" y="185">CYBERSECURITY · SOC · BLUE TEAM</text>
  <text class="term" x="30" y="235">&gt; hello, friend.<tspan class="cur">█</tspan></text>
  <rect class="bar" x="0" y="0" width="900" height="2" opacity=".4"/>
  <rect class="scan" width="900" height="260"/>
</svg>
name: profile-art
on:
  schedule: [{ cron: "0 3 * * *" }]
  workflow_dispatch:
permissions:
  contents: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: yoshi389111/github-profile-3d-contrib@latest
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: ${{ github.repository_owner }}
      - uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg?palette=github-dark&color_snake=#ff0033&color_dots=#161b22,#330000,#660000,#aa0011,#ff0033
      - run: |
          mkdir -p profile-3d-contrib && cp dist/snake.svg profile-3d-contrib/
          git config user.name "fsociety-bot"
          git config user.email "bot@users.noreply.github.com"
          git add -A && git commit -m "update profile art" || true
          git push
