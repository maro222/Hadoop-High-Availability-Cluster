# Hadoop High Availability (HA) Cluster

> **ITI Project** — Under the supervision of **Eng. Youssef Etman**

---

## Overview

This project sets up a fully functional **Hadoop High Availability cluster** using Docker containers. It covers HDFS HA, YARN HA, ZooKeeper quorum, and JournalNodes — with automatic failover enabled.

---

## Cluster Architecture

| Node | Roles |
|------|-------|
| **master1** | Active NameNode, ResourceManager (rm1), JournalNode, ZooKeeper, ZKFC |
| **master2** | Standby NameNode, ResourceManager (rm2), JournalNode, ZooKeeper, ZKFC |
| **worker1** | DataNode, NodeManager, JournalNode, ZooKeeper |
| **worker2** | DataNode, NodeManager |
| **worker3** | DataNode, NodeManager |

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1100 920" width="1100" height="920" font-family="'Courier New', monospace">
  <defs>
    <!-- Background gradient -->
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0a0f1e"/>
      <stop offset="100%" style="stop-color:#0d1628"/>
    </linearGradient>

    <!-- Node gradients -->
    <linearGradient id="masterActiveGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0d2a3a"/>
      <stop offset="100%" style="stop-color:#0a1f2e"/>
    </linearGradient>
    <linearGradient id="masterStandbyGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#2a1f0d"/>
      <stop offset="100%" style="stop-color:#1f170a"/>
    </linearGradient>
    <linearGradient id="workerGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#0d2a1a"/>
      <stop offset="100%" style="stop-color:#0a1f12"/>
    </linearGradient>

    <!-- Arrow markers -->
    <marker id="arrowCyan" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#00e5ff"/>
    </marker>
    <marker id="arrowOrange" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#ff9d00"/>
    </marker>
    <marker id="arrowPurple" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#bf5fff"/>
    </marker>
    <marker id="arrowPink" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#ff4f7b"/>
    </marker>
    <marker id="arrowGreen" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#39ff7a"/>
    </marker>
    <marker id="arrowYellow" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L8,3 L0,6 Z" fill="#ffdd57"/>
    </marker>
    <marker id="arrowCyanBack" markerWidth="8" markerHeight="8" refX="2" refY="3" orient="auto">
      <path d="M8,0 L0,3 L8,6 Z" fill="#00e5ff"/>
    </marker>
    <marker id="arrowGreenBack" markerWidth="8" markerHeight="8" refX="2" refY="3" orient="auto">
      <path d="M8,0 L0,3 L8,6 Z" fill="#39ff7a"/>
    </marker>

    <!-- Glow filters -->
    <filter id="glowCyan">
      <feGaussianBlur stdDeviation="3" result="blur"/>
      <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
    <filter id="glowPurple">
      <feGaussianBlur stdDeviation="2" result="blur"/>
      <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
  </defs>

  <!-- ══════════════════ BACKGROUND ══════════════════ -->
  <rect width="1100" height="920" fill="url(#bg)" rx="12"/>

  <!-- subtle grid -->
  <g opacity="0.04" stroke="#4488ff" stroke-width="0.5">
    <line x1="0" y1="100" x2="1100" y2="100"/>
    <line x1="0" y1="200" x2="1100" y2="200"/>
    <line x1="0" y1="300" x2="1100" y2="300"/>
    <line x1="0" y1="400" x2="1100" y2="400"/>
    <line x1="0" y1="500" x2="1100" y2="500"/>
    <line x1="0" y1="600" x2="1100" y2="600"/>
    <line x1="0" y1="700" x2="1100" y2="700"/>
    <line x1="0" y1="800" x2="1100" y2="800"/>
    <line x1="100" y1="0" x2="100" y2="920"/>
    <line x1="200" y1="0" x2="200" y2="920"/>
    <line x1="300" y1="0" x2="300" y2="920"/>
    <line x1="400" y1="0" x2="400" y2="920"/>
    <line x1="500" y1="0" x2="500" y2="920"/>
    <line x1="600" y1="0" x2="600" y2="920"/>
    <line x1="700" y1="0" x2="700" y2="920"/>
    <line x1="800" y1="0" x2="800" y2="920"/>
    <line x1="900" y1="0" x2="900" y2="920"/>
    <line x1="1000" y1="0" x2="1000" y2="920"/>
  </g>

  <!-- ══════════════════ TITLE ══════════════════ -->
  <text x="550" y="38" text-anchor="middle" fill="#00e5ff" font-size="18" font-weight="bold" letter-spacing="4">HADOOP HIGH AVAILABILITY CLUSTER</text>
  <text x="550" y="56" text-anchor="middle" fill="#4a6080" font-size="10" letter-spacing="3">ITI PROJECT · ENG. YOUSSEF ETMAN · HADOOP 3.4.2 · ZOOKEEPER 3.8.6</text>

  <!-- ══════════════════ QUORUM BANNERS ══════════════════ -->
  <!-- ZooKeeper quorum band -->
  <rect x="30" y="68" width="1040" height="26" rx="5" fill="#bf5fff08" stroke="#bf5fff" stroke-width="1" stroke-dasharray="5,3"/>
  <text x="550" y="85" text-anchor="middle" fill="#bf5fff" font-size="10" letter-spacing="2">⬡  ZOOKEEPER QUORUM  —  master1:2181  ·  master2:2181  ·  worker1:2181  ⬡</text>

  <!-- JournalNode quorum band -->
  <rect x="30" y="99" width="1040" height="26" rx="5" fill="#ff4f7b08" stroke="#ff4f7b" stroke-width="1" stroke-dasharray="5,3"/>
  <text x="550" y="116" text-anchor="middle" fill="#ff4f7b" font-size="10" letter-spacing="2">◈  JOURNALNODE QUORUM  —  master1:8485  ·  master2:8485  ·  worker1:8485  ◈</text>

  <!-- ══════════════════════════════════════════════════════
       CONNECTIONS  (drawn before nodes so nodes appear on top)
  ══════════════════════════════════════════════════════ -->

  <!-- ── ZKFC ↔ ZooKeeper (master1 ZKFC → ZK cluster) ── -->
  <!-- master1 ZKFC to ZK quorum label -->
  <path d="M 205 310 Q 205 340 205 365" stroke="#00cfff" stroke-width="1.5" stroke-dasharray="4,2" fill="none" marker-end="url(#arrowCyan)"/>
  <text x="148" y="342" fill="#00cfff" font-size="8" letter-spacing="1">ZK LOCK</text>

  <!-- master2 ZKFC to ZK quorum -->
  <path d="M 755 310 Q 755 340 755 365" stroke="#00cfff" stroke-width="1.5" stroke-dasharray="4,2" fill="none" marker-end="url(#arrowCyan)"/>
  <text x="762" y="342" fill="#00cfff" font-size="8" letter-spacing="1">MONITOR</text>

  <!-- ── ZKFC ↔ ZKFC failover line (top, between masters) ── -->
  <path d="M 310 280 Q 550 250 750 280" stroke="#00cfff" stroke-width="2" stroke-dasharray="6,3" fill="none" marker-end="url(#arrowCyan)" marker-start="url(#arrowCyanBack)"/>
  <text x="550" y="248" text-anchor="middle" fill="#00cfff" font-size="9" letter-spacing="1">ZKFC AUTO-FAILOVER</text>
  <text x="550" y="260" text-anchor="middle" fill="#4a6080" font-size="8">( ActiveStandbyElectorLock )</text>

  <!-- ── NameNode ↔ NameNode metadata sync via JournalNodes ── -->
  <!-- Active NN → JN quorum (master1 → master1 JN, master2 JN, worker1 JN) -->
  <!-- master1 NN writes to master1 JN (internal, short) -->
  <path d="M 205 218 L 205 365" stroke="#ff4f7b" stroke-width="1.5" fill="none" marker-end="url(#arrowPink)"/>

  <!-- master1 NN writes to master2 JN -->
  <path d="M 255 205 Q 550 185 745 368" stroke="#ff4f7b" stroke-width="1.5" fill="none" marker-end="url(#arrowPink)"/>
  <text x="430" y="183" fill="#ff4f7b" font-size="8" letter-spacing="1">WRITE EDIT LOGS</text>

  <!-- master1 NN writes to worker1 JN -->
  <path d="M 240 222 Q 350 590 453 618" stroke="#ff4f7b" stroke-width="1.2" stroke-dasharray="3,2" fill="none" marker-end="url(#arrowPink)"/>

  <!-- Standby NN reads from JournalNodes -->
  <path d="M 745 205 Q 550 190 305 370" stroke="#ff9d00" stroke-width="1.5" stroke-dasharray="5,2" fill="none" marker-end="url(#arrowOrange)"/>
  <text x="680" y="183" fill="#ff9d00" font-size="8" letter-spacing="1">READ EDIT LOGS</text>

  <!-- ── NameNode ↔ DataNode block reports ── -->
  <!-- master1 NN ← worker1 DN block report -->
  <path d="M 453 618 Q 350 500 210 230" stroke="#39ff7a" stroke-width="1.2" stroke-dasharray="4,2" fill="none" marker-end="url(#arrowGreen)"/>

  <!-- master1 NN ← worker2 DN block report -->
  <path d="M 660 618 Q 480 480 215 230" stroke="#39ff7a" stroke-width="1.2" stroke-dasharray="4,2" fill="none" marker-end="url(#arrowGreen)"/>

  <!-- master1 NN ← worker3 DN block report -->
  <path d="M 870 618 Q 640 460 220 232" stroke="#39ff7a" stroke-width="1.2" stroke-dasharray="4,2" fill="none" marker-end="url(#arrowGreen)"/>

  <!-- master2 NN ← worker1 DN (standby also receives) -->
  <path d="M 457 620 Q 600 490 745 232" stroke="#39ff7a" stroke-width="0.8" stroke-dasharray="3,3" fill="none" marker-end="url(#arrowGreen)" opacity="0.5"/>

  <!-- ── ResourceManager ↔ NodeManager ── -->
  <!-- master1 RM → worker1 NM -->
  <path d="M 225 248 Q 300 520 453 670" stroke="#ffdd57" stroke-width="1.2" stroke-dasharray="5,2" fill="none" marker-end="url(#arrowYellow)"/>

  <!-- master1 RM → worker2 NM -->
  <path d="M 230 252 Q 420 520 660 670" stroke="#ffdd57" stroke-width="1.2" stroke-dasharray="5,2" fill="none" marker-end="url(#arrowYellow)"/>

  <!-- master1 RM → worker3 NM -->
  <path d="M 235 255 Q 540 510 870 670" stroke="#ffdd57" stroke-width="1.2" stroke-dasharray="5,2" fill="none" marker-end="url(#arrowYellow)"/>
  <text x="300" y="510" fill="#ffdd57" font-size="8" letter-spacing="1" transform="rotate(-68,300,510)">YARN SCHEDULE</text>

  <!-- RM HA failover via ZK -->
  <!-- master2 RM ↔ ZK  -->
  <path d="M 755 248 Q 820 430 755 450" stroke="#ffdd57" stroke-width="1" stroke-dasharray="3,2" fill="none" marker-end="url(#arrowYellow)" opacity="0.6"/>

  <!-- ── ZooKeeper inter-node communication ── -->
  <!-- master1 ZK ↔ master2 ZK leader election -->
  <path d="M 310 430 Q 550 410 750 430" stroke="#bf5fff" stroke-width="1.5" fill="none" marker-end="url(#arrowPurple)" marker-start="url(#arrowPurple)"/>
  <text x="550" y="408" text-anchor="middle" fill="#bf5fff" font-size="8" letter-spacing="1">LEADER ELECTION :3888</text>

  <!-- master1 ZK ↔ worker1 ZK -->
  <path d="M 205 478 Q 205 560 453 635" stroke="#bf5fff" stroke-width="1.2" fill="none" marker-end="url(#arrowPurple)" opacity="0.8"/>

  <!-- master2 ZK ↔ worker1 ZK -->
  <path d="M 755 478 Q 755 560 510 635" stroke="#bf5fff" stroke-width="1.2" fill="none" marker-end="url(#arrowPurple)" opacity="0.8"/>
  <text x="690" y="575" fill="#bf5fff" font-size="8" letter-spacing="0.5">QUORUM SYNC :2888</text>

  <!-- ══════════════════════════════════════════════════════
       MASTER 1 NODE  (Active)
  ══════════════════════════════════════════════════════ -->
  <g>
    <!-- outer container -->
    <rect x="50" y="135" width="310" height="360" rx="14" fill="url(#masterActiveGrad)" stroke="#00e5ff" stroke-width="2"/>
    <!-- header bar -->
    <rect x="50" y="135" width="310" height="40" rx="14" fill="#00e5ff18"/>
    <rect x="50" y="155" width="310" height="20" fill="#00e5ff18"/>
    <text x="78" y="161" fill="#00e5ff" font-size="14" font-weight="bold" letter-spacing="2">master1</text>
    <rect x="220" y="145" width="60" height="18" rx="9" fill="#00e5ff22" stroke="#00e5ff" stroke-width="1"/>
    <text x="250" y="157" text-anchor="middle" fill="#00e5ff" font-size="9" font-weight="bold" letter-spacing="1">ACTIVE</text>
    <text x="78" y="172" fill="#4a6080" font-size="9" letter-spacing="1">master node · docker container</text>

    <!-- NameNode service -->
    <rect x="68" y="182" width="274" height="52" rx="8" fill="#00e5ff0d" stroke="#00e5ff" stroke-width="1"/>
    <text x="82" y="198" fill="#00e5ff" font-size="11" font-weight="bold">NameNode</text>
    <text x="82" y="212" fill="#4a6080" font-size="8">Manages HDFS namespace &amp; metadata</text>
    <rect x="82" y="217" width="42" height="12" rx="3" fill="#00e5ff15" stroke="#00e5ff55" stroke-width="1"/>
    <text x="103" y="226" text-anchor="middle" fill="#00e5ff" font-size="8">:9870</text>
    <rect x="130" y="217" width="42" height="12" rx="3" fill="#00e5ff15" stroke="#00e5ff55" stroke-width="1"/>
    <text x="151" y="226" text-anchor="middle" fill="#00e5ff" font-size="8">:8020</text>

    <!-- ResourceManager -->
    <rect x="68" y="242" width="274" height="52" rx="8" fill="#ffdd5710" stroke="#ffdd57" stroke-width="1"/>
    <text x="82" y="258" fill="#ffdd57" font-size="11" font-weight="bold">ResourceManager  rm1</text>
    <text x="82" y="272" fill="#4a6080" font-size="8">YARN scheduling &amp; resource allocation</text>
    <rect x="82" y="277" width="42" height="12" rx="3" fill="#ffdd5715" stroke="#ffdd5755" stroke-width="1"/>
    <text x="103" y="286" text-anchor="middle" fill="#ffdd57" font-size="8">:8088</text>
    <rect x="130" y="277" width="42" height="12" rx="3" fill="#ffdd5715" stroke="#ffdd5755" stroke-width="1"/>
    <text x="151" y="286" text-anchor="middle" fill="#ffdd57" font-size="8">:8033</text>

    <!-- ZKFC -->
    <rect x="68" y="302" width="274" height="44" rx="8" fill="#00cfff0d" stroke="#00cfff" stroke-width="1"/>
    <text x="82" y="318" fill="#00cfff" font-size="11" font-weight="bold">ZKFC</text>
    <text x="82" y="332" fill="#4a6080" font-size="8">DFSZKFailoverController · holds ZK ActiveLock</text>

    <!-- JournalNode -->
    <rect x="68" y="354" width="130" height="44" rx="8" fill="#ff4f7b0d" stroke="#ff4f7b" stroke-width="1"/>
    <text x="82" y="370" fill="#ff4f7b" font-size="11" font-weight="bold">JournalNode</text>
    <text x="82" y="384" fill="#4a6080" font-size="8">Stores HDFS edit logs</text>
    <rect x="82" y="387" width="42" height="10" rx="3" fill="#ff4f7b15" stroke="#ff4f7b55" stroke-width="1"/>
    <text x="103" y="394" text-anchor="middle" fill="#ff4f7b" font-size="7">:8485</text>

    <!-- ZooKeeper -->
    <rect x="210" y="354" width="132" height="44" rx="8" fill="#bf5fff0d" stroke="#bf5fff" stroke-width="1"/>
    <text x="224" y="370" fill="#bf5fff" font-size="11" font-weight="bold">ZooKeeper</text>
    <text x="224" y="381" fill="#4a6080" font-size="8">server.1 · myid=1</text>
    <rect x="224" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="239" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:2181</text>
    <rect x="258" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="273" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:2888</text>
    <rect x="292" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="307" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:3888</text>

    <!-- QuorumPeerMain label -->
    <text x="78" y="490" fill="#4a6080" font-size="8">QuorumPeerMain</text>
  </g>

  <!-- ══════════════════════════════════════════════════════
       MASTER 2 NODE  (Standby)
  ══════════════════════════════════════════════════════ -->
  <g>
    <rect x="740" y="135" width="310" height="360" rx="14" fill="url(#masterStandbyGrad)" stroke="#ff9d00" stroke-width="2"/>
    <rect x="740" y="135" width="310" height="40" rx="14" fill="#ff9d0018"/>
    <rect x="740" y="155" width="310" height="20" fill="#ff9d0018"/>
    <text x="768" y="161" fill="#ff9d00" font-size="14" font-weight="bold" letter-spacing="2">master2</text>
    <rect x="910" y="145" width="68" height="18" rx="9" fill="#ff9d0022" stroke="#ff9d00" stroke-width="1"/>
    <text x="944" y="157" text-anchor="middle" fill="#ff9d00" font-size="9" font-weight="bold" letter-spacing="1">STANDBY</text>
    <text x="768" y="172" fill="#4a6080" font-size="9" letter-spacing="1">master node · docker container</text>

    <!-- NameNode -->
    <rect x="758" y="182" width="274" height="52" rx="8" fill="#ff9d000d" stroke="#ff9d00" stroke-width="1"/>
    <text x="772" y="198" fill="#ff9d00" font-size="11" font-weight="bold">NameNode</text>
    <text x="772" y="212" fill="#4a6080" font-size="8">Syncs from JournalNodes · ready for failover</text>
    <rect x="772" y="217" width="42" height="12" rx="3" fill="#ff9d0015" stroke="#ff9d0055" stroke-width="1"/>
    <text x="793" y="226" text-anchor="middle" fill="#ff9d00" font-size="8">:9870</text>
    <rect x="820" y="217" width="42" height="12" rx="3" fill="#ff9d0015" stroke="#ff9d0055" stroke-width="1"/>
    <text x="841" y="226" text-anchor="middle" fill="#ff9d00" font-size="8">:8020</text>

    <!-- ResourceManager -->
    <rect x="758" y="242" width="274" height="52" rx="8" fill="#ffdd5710" stroke="#ffdd57" stroke-width="1"/>
    <text x="772" y="258" fill="#ffdd57" font-size="11" font-weight="bold">ResourceManager  rm2</text>
    <text x="772" y="272" fill="#4a6080" font-size="8">Standby YARN RM · ZK state store recovery</text>
    <rect x="772" y="277" width="42" height="12" rx="3" fill="#ffdd5715" stroke="#ffdd5755" stroke-width="1"/>
    <text x="793" y="286" text-anchor="middle" fill="#ffdd57" font-size="8">:8088</text>
    <rect x="820" y="277" width="42" height="12" rx="3" fill="#ffdd5715" stroke="#ffdd5755" stroke-width="1"/>
    <text x="841" y="286" text-anchor="middle" fill="#ffdd57" font-size="8">:8033</text>

    <!-- ZKFC -->
    <rect x="758" y="302" width="274" height="44" rx="8" fill="#00cfff0d" stroke="#00cfff" stroke-width="1"/>
    <text x="772" y="318" fill="#00cfff" font-size="11" font-weight="bold">ZKFC</text>
    <text x="772" y="332" fill="#4a6080" font-size="8">Monitors NN health · triggers failover via ZK</text>

    <!-- JournalNode -->
    <rect x="758" y="354" width="130" height="44" rx="8" fill="#ff4f7b0d" stroke="#ff4f7b" stroke-width="1"/>
    <text x="772" y="370" fill="#ff4f7b" font-size="11" font-weight="bold">JournalNode</text>
    <text x="772" y="384" fill="#4a6080" font-size="8">Stores HDFS edit logs</text>
    <rect x="772" y="387" width="42" height="10" rx="3" fill="#ff4f7b15" stroke="#ff4f7b55" stroke-width="1"/>
    <text x="793" y="394" text-anchor="middle" fill="#ff4f7b" font-size="7">:8485</text>

    <!-- ZooKeeper -->
    <rect x="900" y="354" width="132" height="44" rx="8" fill="#bf5fff0d" stroke="#bf5fff" stroke-width="1"/>
    <text x="914" y="370" fill="#bf5fff" font-size="11" font-weight="bold">ZooKeeper</text>
    <text x="914" y="381" fill="#4a6080" font-size="8">server.2 · myid=2</text>
    <rect x="914" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="929" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:2181</text>
    <rect x="948" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="963" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:2888</text>
    <rect x="982" y="384" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="997" y="391" text-anchor="middle" fill="#bf5fff" font-size="7">:3888</text>

    <text x="768" y="490" fill="#4a6080" font-size="8">QuorumPeerMain</text>
  </g>

  <!-- ══════════════════════════════════════════════════════
       WORKER 1 NODE  (JN + ZK + DN + NM)
  ══════════════════════════════════════════════════════ -->
  <g>
    <rect x="340" y="560" width="230" height="300" rx="14" fill="url(#workerGrad)" stroke="#39ff7a" stroke-width="2"/>
    <rect x="340" y="560" width="230" height="38" rx="14" fill="#39ff7a18"/>
    <rect x="340" y="578" width="230" height="20" fill="#39ff7a18"/>
    <text x="362" y="583" fill="#39ff7a" font-size="13" font-weight="bold" letter-spacing="2">worker1</text>
    <text x="362" y="595" fill="#4a6080" font-size="8" letter-spacing="1">worker node · most loaded</text>

    <!-- DataNode -->
    <rect x="356" y="606" width="198" height="42" rx="7" fill="#39ff7a0d" stroke="#39ff7a" stroke-width="1"/>
    <text x="370" y="622" fill="#39ff7a" font-size="11" font-weight="bold">DataNode</text>
    <text x="370" y="636" fill="#4a6080" font-size="8">Stores HDFS blocks · sends heartbeat</text>
    <text x="370" y="643" fill="#4a6080" font-size="8">Block Report to both NNs</text>

    <!-- NodeManager -->
    <rect x="356" y="655" width="198" height="36" rx="7" fill="#ffdd5710" stroke="#ffdd57" stroke-width="1"/>
    <text x="370" y="671" fill="#ffdd57" font-size="11" font-weight="bold">NodeManager</text>
    <text x="370" y="683" fill="#4a6080" font-size="8">Runs YARN containers / tasks</text>

    <!-- JournalNode -->
    <rect x="356" y="698" width="198" height="42" rx="7" fill="#ff4f7b0d" stroke="#ff4f7b" stroke-width="1"/>
    <text x="370" y="714" fill="#ff4f7b" font-size="11" font-weight="bold">JournalNode</text>
    <text x="370" y="728" fill="#4a6080" font-size="8">3rd JN in quorum · edit log ring</text>
    <rect x="370" y="731" width="42" height="10" rx="3" fill="#ff4f7b15" stroke="#ff4f7b55" stroke-width="1"/>
    <text x="391" y="738" text-anchor="middle" fill="#ff4f7b" font-size="7">:8485</text>

    <!-- ZooKeeper -->
    <rect x="356" y="747" width="198" height="50" rx="7" fill="#bf5fff0d" stroke="#bf5fff" stroke-width="1"/>
    <text x="370" y="763" fill="#bf5fff" font-size="11" font-weight="bold">ZooKeeper</text>
    <text x="370" y="775" fill="#4a6080" font-size="8">server.3 · myid=3 · completes quorum</text>
    <rect x="370" y="779" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="385" y="786" text-anchor="middle" fill="#bf5fff" font-size="7">:2181</text>
    <rect x="404" y="779" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="419" y="786" text-anchor="middle" fill="#bf5fff" font-size="7">:2888</text>
    <rect x="438" y="779" width="30" height="10" rx="3" fill="#bf5fff15" stroke="#bf5fff55" stroke-width="1"/>
    <text x="453" y="786" text-anchor="middle" fill="#bf5fff" font-size="7">:3888</text>

    <text x="362" y="853" fill="#4a6080" font-size="8">QuorumPeerMain</text>
  </g>

  <!-- ══════════════════════════════════════════════════════
       WORKER 2 NODE  (DN + NM only)
  ══════════════════════════════════════════════════════ -->
  <g>
    <rect x="590" y="560" width="200" height="175" rx="14" fill="url(#workerGrad)" stroke="#39ff7a" stroke-width="1.5"/>
    <rect x="590" y="560" width="200" height="38" rx="14" fill="#39ff7a18"/>
    <rect x="590" y="578" width="200" height="20" fill="#39ff7a18"/>
    <text x="612" y="583" fill="#39ff7a" font-size="13" font-weight="bold" letter-spacing="2">worker2</text>
    <text x="612" y="595" fill="#4a6080" font-size="8" letter-spacing="1">worker node · data only</text>

    <!-- DataNode -->
    <rect x="606" y="606" width="168" height="52" rx="7" fill="#39ff7a0d" stroke="#39ff7a" stroke-width="1"/>
    <text x="620" y="622" fill="#39ff7a" font-size="11" font-weight="bold">DataNode</text>
    <text x="620" y="636" fill="#4a6080" font-size="8">Stores HDFS blocks</text>
    <text x="620" y="648" fill="#4a6080" font-size="8">Block Report + Heartbeat → NN</text>

    <!-- NodeManager -->
    <rect x="606" y="665" width="168" height="36" rx="7" fill="#ffdd5710" stroke="#ffdd57" stroke-width="1"/>
    <text x="620" y="681" fill="#ffdd57" font-size="11" font-weight="bold">NodeManager</text>
    <text x="620" y="693" fill="#4a6080" font-size="8">Runs YARN containers / tasks</text>

    <text x="612" y="730" fill="#4a6080" font-size="8">No ZK · No JN</text>
  </g>

  <!-- ══════════════════════════════════════════════════════
       WORKER 3 NODE  (DN + NM only)
  ══════════════════════════════════════════════════════ -->
  <g>
    <rect x="810" y="560" width="200" height="175" rx="14" fill="url(#workerGrad)" stroke="#39ff7a" stroke-width="1.5"/>
    <rect x="810" y="560" width="200" height="38" rx="14" fill="#39ff7a18"/>
    <rect x="810" y="578" width="200" height="20" fill="#39ff7a18"/>
    <text x="832" y="583" fill="#39ff7a" font-size="13" font-weight="bold" letter-spacing="2">worker3</text>
    <text x="832" y="595" fill="#4a6080" font-size="8" letter-spacing="1">worker node · data only</text>

    <!-- DataNode -->
    <rect x="826" y="606" width="168" height="52" rx="7" fill="#39ff7a0d" stroke="#39ff7a" stroke-width="1"/>
    <text x="840" y="622" fill="#39ff7a" font-size="11" font-weight="bold">DataNode</text>
    <text x="840" y="636" fill="#4a6080" font-size="8">Stores HDFS blocks</text>
    <text x="840" y="648" fill="#4a6080" font-size="8">Block Report + Heartbeat → NN</text>

    <!-- NodeManager -->
    <rect x="826" y="665" width="168" height="36" rx="7" fill="#ffdd5710" stroke="#ffdd57" stroke-width="1"/>
    <text x="840" y="681" fill="#ffdd57" font-size="11" font-weight="bold">NodeManager</text>
    <text x="840" y="693" fill="#4a6080" font-size="8">Runs YARN containers / tasks</text>

    <text x="832" y="730" fill="#4a6080" font-size="8">No ZK · No JN</text>
  </g>

  <!-- ══════════════════════════════════════════════════════
       LEGEND
  ══════════════════════════════════════════════════════ -->
  <rect x="30" y="760" width="298" height="148" rx="10" fill="#0d1628" stroke="#1a2a4a" stroke-width="1"/>
  <text x="46" y="778" fill="#c9d8f0" font-size="10" font-weight="bold" letter-spacing="2">LEGEND</text>

  <!-- legend items -->
  <line x1="46" y1="793" x2="80" y2="793" stroke="#ff4f7b" stroke-width="2" marker-end="url(#arrowPink)"/>
  <text x="90" y="797" fill="#ff4f7b" font-size="9">NameNode writes edit logs → JournalNodes</text>

  <line x1="46" y1="813" x2="80" y2="813" stroke="#ff9d00" stroke-width="1.5" stroke-dasharray="4,2" marker-end="url(#arrowOrange)"/>
  <text x="90" y="817" fill="#ff9d00" font-size="9">Standby NN reads edit logs ← JournalNodes</text>

  <line x1="46" y1="833" x2="80" y2="833" stroke="#39ff7a" stroke-width="1.5" stroke-dasharray="4,2" marker-end="url(#arrowGreen)"/>
  <text x="90" y="837" fill="#39ff7a" font-size="9">DataNode block report + heartbeat → NN</text>

  <line x1="46" y1="853" x2="80" y2="853" stroke="#ffdd57" stroke-width="1.5" stroke-dasharray="4,2" marker-end="url(#arrowYellow)"/>
  <text x="90" y="857" fill="#ffdd57" font-size="9">YARN task scheduling RM → NodeManagers</text>

  <line x1="46" y1="873" x2="80" y2="873" stroke="#bf5fff" stroke-width="1.5" marker-end="url(#arrowPurple)" marker-start="url(#arrowPurple)"/>
  <text x="90" y="877" fill="#bf5fff" font-size="9">ZooKeeper quorum sync :2888 / election :3888</text>

  <line x1="46" y1="893" x2="80" y2="893" stroke="#00cfff" stroke-width="1.5" stroke-dasharray="5,2" marker-end="url(#arrowCyan)" marker-start="url(#arrowCyanBack)"/>
  <text x="90" y="897" fill="#00cfff" font-size="9">ZKFC auto-failover via ZooKeeper lock</text>

  <!-- ══════════════════════════════════════════════════════
       STARTUP ORDER
  ══════════════════════════════════════════════════════ -->
  <rect x="345" y="870" width="730" height="42" rx="8" fill="#0d1628" stroke="#1a2a4a" stroke-width="1"/>
  <text x="360" y="885" fill="#4a6080" font-size="8" letter-spacing="2">STARTUP ORDER</text>

  <!-- steps -->
  <g>
    <!-- 1 ZK -->
    <circle cx="395" cy="900" r="10" fill="#bf5fff15" stroke="#bf5fff" stroke-width="1.5"/>
    <text x="395" y="904" text-anchor="middle" fill="#bf5fff" font-size="9" font-weight="bold">1</text>
    <text x="395" y="915" text-anchor="middle" fill="#bf5fff" font-size="7">ZooKeeper</text>
    <line x1="405" y1="900" x2="428" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 2 JN -->
    <circle cx="438" cy="900" r="10" fill="#ff4f7b15" stroke="#ff4f7b" stroke-width="1.5"/>
    <text x="438" y="904" text-anchor="middle" fill="#ff4f7b" font-size="9" font-weight="bold">2</text>
    <text x="438" y="915" text-anchor="middle" fill="#ff4f7b" font-size="7">JournalNode</text>
    <line x1="448" y1="900" x2="471" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 3 format ZK -->
    <circle cx="481" cy="900" r="10" fill="#00cfff15" stroke="#00cfff" stroke-width="1.5"/>
    <text x="481" y="904" text-anchor="middle" fill="#00cfff" font-size="9" font-weight="bold">3</text>
    <text x="481" y="915" text-anchor="middle" fill="#00cfff" font-size="7">fmt ZK</text>
    <line x1="491" y1="900" x2="514" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 4 format NN -->
    <circle cx="524" cy="900" r="10" fill="#00e5ff15" stroke="#00e5ff" stroke-width="1.5"/>
    <text x="524" y="904" text-anchor="middle" fill="#00e5ff" font-size="9" font-weight="bold">4</text>
    <text x="524" y="915" text-anchor="middle" fill="#00e5ff" font-size="7">fmt NN</text>
    <line x1="534" y1="900" x2="557" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 5 start active NN -->
    <circle cx="567" cy="900" r="10" fill="#00e5ff15" stroke="#00e5ff" stroke-width="1.5"/>
    <text x="567" y="904" text-anchor="middle" fill="#00e5ff" font-size="9" font-weight="bold">5</text>
    <text x="567" y="915" text-anchor="middle" fill="#00e5ff" font-size="7">start NN1</text>
    <line x1="577" y1="900" x2="600" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 6 bootstrap standby -->
    <circle cx="610" cy="900" r="10" fill="#ff9d0015" stroke="#ff9d00" stroke-width="1.5"/>
    <text x="610" y="904" text-anchor="middle" fill="#ff9d00" font-size="9" font-weight="bold">6</text>
    <text x="610" y="915" text-anchor="middle" fill="#ff9d00" font-size="7">bootstrap</text>
    <line x1="620" y1="900" x2="643" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 7 ZKFC -->
    <circle cx="653" cy="900" r="10" fill="#00cfff15" stroke="#00cfff" stroke-width="1.5"/>
    <text x="653" y="904" text-anchor="middle" fill="#00cfff" font-size="9" font-weight="bold">7</text>
    <text x="653" y="915" text-anchor="middle" fill="#00cfff" font-size="7">ZKFC</text>
    <line x1="663" y1="900" x2="686" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 8 YARN RM -->
    <circle cx="696" cy="900" r="10" fill="#ffdd5715" stroke="#ffdd57" stroke-width="1.5"/>
    <text x="696" y="904" text-anchor="middle" fill="#ffdd57" font-size="9" font-weight="bold">8</text>
    <text x="696" y="915" text-anchor="middle" fill="#ffdd57" font-size="7">YARN RM</text>
    <line x1="706" y1="900" x2="729" y2="900" stroke="#1a2a4a" stroke-width="1"/>
    <!-- 9 DataNode/NM -->
    <circle cx="739" cy="900" r="10" fill="#39ff7a15" stroke="#39ff7a" stroke-width="1.5"/>
    <text x="739" y="904" text-anchor="middle" fill="#39ff7a" font-size="9" font-weight="bold">9</text>
    <text x="739" y="915" text-anchor="middle" fill="#39ff7a" font-size="7">DN + NM</text>
  </g>

</svg>

---

## Prerequisites

### 1. Java JDK
```bash
sudo apt update
sudo apt install openjdk-8-jdk -y
```

### 2. SSH & PDSH
```bash
sudo apt-get install ssh pdsh
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

### 3. Hadoop
```bash
wget https://archive.apache.org/dist/hadoop/core/hadoop-3.4.2/hadoop-3.4.2.tar.gz
tar -xzf hadoop-3.4.2.tar.gz
```

### 4. ZooKeeper
```bash
wget https://dlcdn.apache.org/zookeeper/zookeeper-3.8.6/apache-zookeeper-3.8.6-bin.tar.gz
tar -zxvf apache-zookeeper-3.8.6-bin.tar.gz -C /
```

---

## Configuration Files

| File | Purpose |
|------|---------|
| `/etc/environment` | System-wide `JAVA_HOME` |
| `/hadoop/etc/hadoop/hadoop-env.sh` | Hadoop daemon environment variables |
| `/etc/profile.d/hadoop-env.sh` | Hadoop + ZooKeeper exports for all users |
| `core-site.xml` | Default FS, ZooKeeper quorum address |
| `hdfs-site.xml` | NameNode HA, JournalNodes, fencing, replication |
| `yarn-site.xml` | ResourceManager HA, ZooKeeper state store |
| `zoo.cfg` | ZooKeeper ensemble configuration |

---

## Startup Guide

### Phase 1 — Foundation Services (all nodes)
```bash
# Start ZooKeeper
zkServer.sh start

# Start JournalNodes
hdfs --daemon start journalnode
```

### Phase 2 — HDFS HA Initialization (run once on fresh cluster)
```bash
# On master1 only
hdfs zkfc -formatZK
hdfs namenode -format
hdfs --daemon start namenode

# On master2 only
hdfs namenode -bootstrapStandby
hdfs --daemon start namenode

# On master1 and master2
hdfs --daemon start zkfc
```

### Phase 3 — YARN HA
```bash
# On master1 only (once)
yarn resourcemanager -format-state-store
yarn --daemon start resourcemanager

# On master2
yarn --daemon start resourcemanager
```

### Phase 4 — Worker Services (worker nodes)
```bash
hdfs --daemon start datanode
yarn --daemon start nodemanager
```

---

## Verification

```bash
# Check HDFS HA state
hdfs haadmin -getAllServiceState
# Expected: master1 → active | master2 → standby

# Check YARN HA state
yarn rmadmin -getAllServiceState
# Expected: rm1 → active | rm2 → standby
```

---

## Key Design Decisions

- **Fencing method:** `shell(/bin/true)` (fake fence) — chosen after `sshfence` issues caused split-brain scenarios in Docker networking.
- **ZooKeeper `myid` file:** Placed in `dataDir` on each node to uniquely identify servers in the ensemble.
- **Automatic failover:** Enabled via `dfs.ha.automatic-failover.enabled=true` and ZKFC daemons on both master nodes.
- **YARN state store:** Persisted in ZooKeeper using `ZKRMStateStore` for RM HA recovery.

---

## Documentation

| Document | Description |
|----------|-------------|
| `Overview_of_HA_cluster.pdf` | Architecture overview, HA concepts, fencing, and ZooKeeper role |
| `Setup_for_HA_HDFS_Cluster.pdf` | Prerequisites installation steps |
| `Configure_HA_Hadoop.pdf` | Hadoop config files, startup sequence, and troubleshooting |
| `Configure_HA_Zookeeper.pdf` | ZooKeeper `zoo.cfg` settings and `zkfc` commands |

---

## Important Notes

- ⚠️ **Never re-run formatting** (`hdfs namenode -format` or `hdfs zkfc -formatZK`) on a running cluster — it deletes all metadata.
- `bootstrapStandby` must only run **after** the active NameNode is up.
- ZooKeeper and JournalNodes **must be running** before any HDFS formatting.
