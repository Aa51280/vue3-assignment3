<script setup>
import { computed, ref } from 'vue'
import {
  ArrowDownRight,
  ArrowRight,
  Bell,
  Bookmark,
  BookOpen,
  CalendarDays,
  Check,
  ChevronDown,
  CirclePlay,
  Clock3,
  Compass,
  GraduationCap,
  LayoutDashboard,
  Menu,
  Play,
  Search,
  Sparkles,
  Star,
  X,
} from 'lucide-vue-next'

const activeNav = ref('overview')
const activeCategory = ref('全部课程')
const searchQuery = ref('')
const selectedCourse = ref(null)
const notice = ref('')
const menuOpen = ref(false)

const categories = ['全部课程', '设计创意', '编程开发', '商业思维', '人文生活']
const navItems = [
  { id: 'overview', label: '学习概览', icon: LayoutDashboard },
  { id: 'courses', label: '我的课程', icon: BookOpen },
  { id: 'schedule', label: '学习日历', icon: CalendarDays },
  { id: 'saved', label: '我的收藏', icon: Bookmark },
]

const courses = ref([
  {
    id: 1,
    title: '从零开始的 UI 设计系统',
    category: '设计创意',
    instructor: '林知夏',
    role: '产品设计师 · 前字节跳动',
    image: 'https://images.unsplash.com/photo-1558655146-d09347e92766?auto=format&fit=crop&w=1000&q=85',
    lessons: 24,
    duration: '8 小时 20 分',
    rating: '4.9',
    students: '2.4k',
    progress: 68,
    color: 'mint',
    nextLesson: '组件与变体的搭建',
    saved: true,
  },
  {
    id: 2,
    title: '用 JavaScript 做点有趣的事',
    category: '编程开发',
    instructor: '周予安',
    role: '独立开发者 · 开源作者',
    image: 'https://images.unsplash.com/photo-1516321318423-f06f85e504b3?auto=format&fit=crop&w=1000&q=85',
    lessons: 32,
    duration: '12 小时 10 分',
    rating: '4.8',
    students: '1.8k',
    progress: 32,
    color: 'blue',
    nextLesson: '让页面动起来',
    saved: false,
  },
  {
    id: 3,
    title: '把复杂的事讲清楚',
    category: '商业思维',
    instructor: '陈一诺',
    role: '品牌顾问 · TEDx 演讲者',
    image: 'https://images.unsplash.com/photo-1475721027785-f74eccf877e2?auto=format&fit=crop&w=1000&q=85',
    lessons: 18,
    duration: '6 小时 45 分',
    rating: '5.0',
    students: '936',
    progress: 0,
    color: 'orange',
    nextLesson: '从一个好故事开始',
    saved: false,
  },
  {
    id: 4,
    title: '摄影里的光与情绪',
    category: '人文生活',
    instructor: '许闻溪',
    role: '摄影师 · 视觉创作者',
    image: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=1000&q=85',
    lessons: 16,
    duration: '5 小时 30 分',
    rating: '4.9',
    students: '1.2k',
    progress: 0,
    color: 'pink',
    nextLesson: '认识自然光',
    saved: true,
  },
  {
    id: 5,
    title: '用数据讲好产品故事',
    category: '商业思维',
    instructor: '沈知远',
    role: '数据产品负责人',
    image: 'https://images.unsplash.com/photo-1460925895917-afdab827c52f?auto=format&fit=crop&w=1000&q=85',
    lessons: 20,
    duration: '7 小时 15 分',
    rating: '4.8',
    students: '872',
    progress: 0,
    color: 'yellow',
    nextLesson: '找到值得讲的数据',
    saved: false,
  },
  {
    id: 6,
    title: '前端工程师的代码美学',
    category: '编程开发',
    instructor: '顾行之',
    role: '资深前端工程师',
    image: 'https://images.unsplash.com/photo-1498050108023-c5249f4df085?auto=format&fit=crop&w=1000&q=85',
    lessons: 28,
    duration: '10 小时 05 分',
    rating: '4.9',
    students: '1.5k',
    progress: 0,
    color: 'lavender',
    nextLesson: '写出易读的组件',
    saved: false,
  },
])

const schedule = [
  { day: '今天', time: '19:30', title: 'UI 设计系统 · 第 12 课', meta: '林知夏 · 直播答疑', tone: 'green' },
  { day: '明天', time: '20:00', title: 'JavaScript · 第 8 课', meta: '周予安 · 录播更新', tone: 'blue' },
  { day: '周五', time: '14:00', title: '设计作品集工作坊', meta: '林知夏 · 线上工作坊', tone: 'coral' },
]

const currentNav = computed(() => navItems.find((item) => item.id === activeNav.value))
const filteredCourses = computed(() => {
  const query = searchQuery.value.trim().toLowerCase()
  return courses.value.filter((course) => {
    const matchesCategory = activeCategory.value === '全部课程' || course.category === activeCategory.value
    const matchesQuery = !query || `${course.title} ${course.instructor} ${course.category}`.toLowerCase().includes(query)
    const matchesNav = activeNav.value === 'courses'
      ? course.progress > 0
      : activeNav.value === 'saved'
        ? course.saved
        : true
    return matchesCategory && matchesQuery && matchesNav
  })
})
const inProgressCount = computed(() => courses.value.filter((course) => course.progress > 0).length)
const savedCount = computed(() => courses.value.filter((course) => course.saved).length)

function selectNav(id) {
  activeNav.value = id
  activeCategory.value = '全部课程'
  menuOpen.value = false
}

function toggleSaved(course) {
  course.saved = !course.saved
  notice.value = course.saved ? '已加入收藏' : '已取消收藏'
  window.setTimeout(() => { notice.value = '' }, 2000)
}

function openCourse(course) {
  selectedCourse.value = course
}

function startLesson() {
  if (selectedCourse.value && selectedCourse.value.progress === 0) {
    selectedCourse.value.progress = 5
  }
  selectedCourse.value = null
  notice.value = '已为你打开课程，学习进度已更新'
  window.setTimeout(() => { notice.value = '' }, 2500)
}
</script>

<template>
  <div class="app-shell">
    <aside class="sidebar" :class="{ 'sidebar-open': menuOpen }">
      <a class="brand" href="#top" @click.prevent="selectNav('overview')">
        <span class="brand-mark"><GraduationCap :size="22" :stroke-width="2.2" /></span>
        <span>云阶<span class="brand-light">课堂</span></span>
      </a>

      <div class="side-label">学习空间</div>
      <nav class="side-nav" aria-label="主导航">
        <button
          v-for="item in navItems"
          :key="item.id"
          class="nav-link"
          :class="{ active: activeNav === item.id }"
          @click="selectNav(item.id)"
        >
          <component :is="item.icon" :size="18" :stroke-width="1.8" />
          <span>{{ item.label }}</span>
          <span v-if="item.id === 'courses'" class="nav-count">{{ inProgressCount }}</span>
          <span v-if="item.id === 'saved'" class="nav-count">{{ savedCount }}</span>
        </button>
      </nav>

      <div class="sidebar-spacer"></div>

      <div class="streak-card">
        <div class="streak-heading"><span class="streak-sun"><Sparkles :size="16" /></span><span>学习正当时</span></div>
        <div class="streak-number">连续学习 <strong>6</strong> 天</div>
        <div class="streak-dots" aria-label="本周已学习六天">
          <span v-for="(day, index) in ['一', '二', '三', '四', '五', '六', '日']" :key="day" :class="{ done: index < 6 }">{{ day }}</span>
        </div>
      </div>

      <button class="profile-button" @click="notice = '个人资料功能即将开放'">
        <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=96&h=96&q=80" alt="林同学头像" />
        <span class="profile-info"><strong>林同学</strong><small>探索者 · Lv. 3</small></span>
        <ChevronDown :size="16" />
      </button>
    </aside>

    <div v-if="menuOpen" class="mobile-scrim" @click="menuOpen = false"></div>

    <main id="top" class="main-area">
      <header class="topbar">
        <button class="icon-button menu-button" aria-label="打开导航" @click="menuOpen = !menuOpen"><Menu :size="20" /></button>
        <div class="breadcrumb"><span>学习空间</span><ArrowRight :size="14" /><strong>{{ currentNav?.label }}</strong></div>
        <div class="top-actions">
          <label class="search-box">
            <Search :size="17" />
            <input v-model="searchQuery" type="search" placeholder="搜课程、老师或关键词" aria-label="搜索课程" />
            <kbd>⌘ K</kbd>
          </label>
          <button class="icon-button notification-button" aria-label="通知" @click="notice = '暂时没有新的通知'"><Bell :size="19" /><i></i></button>
          <img class="top-avatar" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=96&h=96&q=80" alt="" />
        </div>
      </header>

      <div class="page-content">
        <template v-if="activeNav === 'schedule'">
          <section class="page-heading schedule-heading">
            <div><p class="eyebrow">MAKE ROOM FOR LEARNING</p><h1>学习日历<span class="heading-period">.</span></h1><p class="heading-copy">把好奇心放进日程里，进步自然会发生。</p></div>
            <button class="outline-button" @click="notice = '日历已同步到本周视图'"><CalendarDays :size="16" /> 本周日程 <ChevronDown :size="15" /></button>
          </section>
          <section class="calendar-layout">
            <div class="calendar-main">
              <div class="calendar-toolbar"><button class="calendar-arrow" aria-label="上周" @click="notice = '已显示本周日程'">‹</button><strong>11 月 9 日 — 15 日</strong><button class="calendar-arrow" aria-label="下周" @click="notice = '下周日程即将开放'">›</button><span class="calendar-month">2026 年</span></div>
              <div class="week-grid">
                <div v-for="(day, index) in ['周一', '周二', '周三', '周四', '周五', '周六', '周日']" :key="day" class="week-day" :class="{ 'today-column': index === 0 }">
                  <span>{{ day }}</span><strong>{{ 9 + index }}</strong>
                  <div v-if="index === 0" class="day-event green-event"><small>19:30</small><b>UI 设计系统</b></div>
                  <div v-if="index === 1" class="day-event blue-event"><small>20:00</small><b>JavaScript</b></div>
                  <div v-if="index === 4" class="day-event coral-event"><small>14:00</small><b>作品集工作坊</b></div>
                </div>
              </div>
            </div>
            <aside class="upcoming-panel"><div class="panel-title"><h2>接下来</h2><span>本周 3 场</span></div><article v-for="item in schedule" :key="item.title" class="upcoming-item"><span class="event-date" :class="item.tone">{{ item.day }}</span><div><strong>{{ item.title }}</strong><p><Clock3 :size="13" /> {{ item.time }} <span>·</span> {{ item.meta }}</p></div><ArrowRight :size="16" /></article><button class="text-action" @click="selectNav('courses')">去我的课程 <ArrowRight :size="15" /></button></aside>
          </section>
        </template>

        <template v-else>
          <section class="welcome-row">
            <div class="welcome-copy"><p class="eyebrow">MONDAY, NOVEMBER 09, 2026 <span class="eyebrow-dot"></span> 第 45 周</p><h1>{{ activeNav === 'courses' ? '我的学习' : activeNav === 'saved' ? '灵感收藏夹' : '早上好，林同学' }}<span class="heading-period">.</span></h1><p class="heading-copy">{{ activeNav === 'courses' ? '每一步都算数，继续保持你的节奏。' : activeNav === 'saved' ? '把喜欢的课程收好，灵感随时可以开始。' : '今天也留一点时间，给自己新的可能。' }}</p></div>
            <div class="week-progress"><div class="week-progress-label"><span>本周学习目标</span><strong>4.5 <small>/ 6 小时</small></strong></div><div class="progress-track"><span style="width: 75%"></span></div><span class="progress-caption"><Check :size="13" /> 已完成本周目标的 75%</span></div>
          </section>

          <section v-if="activeNav === 'overview'" class="feature-banner">
            <div class="banner-art"><div class="art-ring ring-one"></div><div class="art-ring ring-two"></div><div class="art-sun"></div><div class="art-sparkle sparkle-one">✳</div><div class="art-sparkle sparkle-two">✦</div><div class="art-stamp">KEEP<br />GROWING</div></div>
            <div class="banner-content"><span class="banner-kicker"><Sparkles :size="14" /> 为你挑选</span><h2>把好奇心，<br />变成下一项能力。</h2><p>从一个小小的开始，走向更大的可能。</p><button class="banner-button" @click="document.getElementById('course-catalog')?.scrollIntoView({ behavior: 'smooth' })">发现新课程 <ArrowDownRight :size="16" /></button></div>
            <div class="banner-index">01 <span>/ 03</span></div>
          </section>

          <section v-if="activeNav === 'overview'" class="continue-section">
            <div class="section-heading"><div><p class="eyebrow">PICK UP WHERE YOU LEFT OFF</p><h2>接着上次学 <span>↗</span></h2></div><button class="text-action" @click="selectNav('courses')">全部学习中 <ArrowRight :size="15" /></button></div>
            <div class="continue-card" v-if="courses.find((course) => course.progress > 0)">
              <div class="continue-image"><img :src="courses.find((course) => course.progress > 0).image" alt="课程封面" /><button aria-label="继续播放" @click="openCourse(courses.find((course) => course.progress > 0))"><Play :size="17" fill="currentColor" /></button></div>
              <div class="continue-info"><span class="mini-label">正在学习 <i></i> {{ courses.find((course) => course.progress > 0).category }}</span><h3>{{ courses.find((course) => course.progress > 0).title }}</h3><p>下一节：{{ courses.find((course) => course.progress > 0).nextLesson }}</p><div class="continue-progress"><div class="progress-track"><span :style="{ width: `${courses.find((course) => course.progress > 0).progress}%` }"></span></div><span>{{ courses.find((course) => course.progress > 0).progress }}%</span></div></div>
              <button class="continue-action" aria-label="继续学习" @click="openCourse(courses.find((course) => course.progress > 0))"><ArrowRight :size="19" /></button>
            </div>
          </section>

          <section id="course-catalog" class="catalog-section">
            <div class="section-heading catalog-heading"><div><p class="eyebrow">A LITTLE CURIOSITY GOES A LONG WAY</p><h2>{{ activeNav === 'courses' ? '正在学习' : activeNav === 'saved' ? '收藏的课程' : '为你找到好课' }} <span>↗</span></h2></div><button class="sort-button" @click="notice = '当前按推荐顺序排列'">推荐排序 <ChevronDown :size="15" /></button></div>
            <div class="category-row" role="group" aria-label="课程分类"><button v-for="category in categories" :key="category" class="category-chip" :class="{ selected: activeCategory === category }" @click="activeCategory = category">{{ category }}<span v-if="activeCategory === category" class="chip-dot"></span></button><span class="result-count">{{ filteredCourses.length }} 门课程</span></div>

            <div v-if="filteredCourses.length" class="course-grid">
              <article v-for="course in filteredCourses" :key="course.id" class="course-card">
                <button class="course-cover" :class="course.color" @click="openCourse(course)" :aria-label="`查看课程：${course.title}`"><img :src="course.image" :alt="course.title" loading="lazy" /><span class="cover-category">{{ course.category }}</span><span v-if="course.progress > 0" class="cover-progress">学习中 {{ course.progress }}%</span><span class="cover-play"><Play :size="16" fill="currentColor" /></span></button>
                <div class="course-body"><div class="course-title-row"><button class="course-title" @click="openCourse(course)">{{ course.title }}</button><button class="save-button" :class="{ saved: course.saved }" :aria-label="course.saved ? '取消收藏' : '收藏课程'" @click="toggleSaved(course)"><Bookmark :size="17" :fill="course.saved ? 'currentColor' : 'none'" /></button></div><div class="instructor-row"><span class="instructor-avatar">{{ course.instructor.slice(0, 1) }}</span><span>{{ course.instructor }}</span><span class="instructor-divider">·</span><span class="instructor-role">{{ course.role.split(' · ')[0] }}</span></div><div class="course-meta"><span><Star :size="14" fill="currentColor" /> {{ course.rating }}</span><span><CirclePlay :size="14" /> {{ course.lessons }} 节</span><span><Clock3 :size="14" /> {{ course.duration }}</span></div></div>
              </article>
            </div>
            <div v-else class="empty-state"><span class="empty-icon"><Compass :size="23" /></span><h3>{{ searchQuery ? '没有找到相关课程' : activeNav === 'saved' ? '收藏夹还是空的' : '这里暂时没有课程' }}</h3><p>{{ searchQuery ? '换个关键词试试看。' : '试试其他分类，发现新的兴趣方向。' }}</p><button @click="activeCategory = '全部课程'; searchQuery = ''; activeNav = 'overview'">浏览全部课程 <ArrowRight :size="15" /></button></div>
            <button v-if="activeNav === 'overview'" class="load-more" @click="notice = '已经展示全部精选课程'">已经到底啦 <span></span></button>
          </section>
        </template>

        <footer class="page-footer"><span>© 2026 云阶课堂</span><span>让学习成为日常 <i>✳</i></span><a href="mailto:hello@yunclass.example">需要帮助？</a></footer>
      </div>
    </main>

    <Transition name="toast"><div v-if="notice" class="toast-message"><Check :size="16" /> {{ notice }}</div></Transition>

    <Transition name="modal">
      <div v-if="selectedCourse" class="modal-backdrop" @click.self="selectedCourse = null" @keydown.esc="selectedCourse = null">
        <section class="course-modal" role="dialog" aria-modal="true" :aria-label="selectedCourse.title">
          <button class="modal-close" aria-label="关闭" @click="selectedCourse = null"><X :size="19" /></button>
          <div class="modal-cover"><img :src="selectedCourse.image" :alt="selectedCourse.title" /><span>{{ selectedCourse.category }}</span></div>
          <div class="modal-content"><p class="eyebrow">A COURSE FOR YOUR NEXT CHAPTER</p><h2>{{ selectedCourse.title }}</h2><p class="modal-instructor">{{ selectedCourse.instructor }} · {{ selectedCourse.role }}</p><p class="modal-description">用清晰的路径和丰富的练习，把灵感变成真正带得走的能力。课程包含 {{ selectedCourse.lessons }} 节内容，共 {{ selectedCourse.duration }}，可随时回看。</p><div class="modal-stats"><span><Star :size="15" fill="currentColor" /> {{ selectedCourse.rating }} 课程评分</span><span><BookOpen :size="15" /> {{ selectedCourse.lessons }} 节课程</span></div><div class="modal-next"><span class="next-play"><Play :size="14" fill="currentColor" /></span><span><small>{{ selectedCourse.progress ? '继续学习' : '第一节课程' }}</small><strong>{{ selectedCourse.progress ? selectedCourse.nextLesson : '认识这门课程' }}</strong></span><span class="next-duration">12 分钟</span></div><button class="modal-start" @click="startLesson">{{ selectedCourse.progress ? '继续学习' : '开始学习' }} <ArrowRight :size="17" /></button></div>
        </section>
      </div>
    </Transition>
  </div>
</template>