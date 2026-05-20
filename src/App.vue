<script setup>
import { computed, onBeforeUnmount, onMounted, reactive, ref } from "vue";

const isScrolled = ref(false);
const isMenuOpen = ref(false);
const formNote = ref("");
const booking = reactive({
  name: "",
  phone: "",
  partySize: "",
  date: "",
});

const today = computed(() => new Date().toISOString().slice(0, 10));

const dishes = [
  {
    tag: "炭火",
    title: "桂花蜜汁炭烤排骨",
    copy: "低温慢烤后上炭炉收香，入口带轻微焦糖与桂花尾韵。",
    image: "https://images.unsplash.com/photo-1544025162-d76694265947?auto=format&fit=crop&w=900&q=84",
    alt: "炭烤肉类主菜",
  },
  {
    tag: "时令",
    title: "莼菜蟹粉豆腐羹",
    copy: "蟹粉鲜甜、豆腐细滑，适合从第一口开始安静下来。",
    image: "https://images.unsplash.com/photo-1569058242253-92a9c755a0ec?auto=format&fit=crop&w=900&q=84",
    alt: "精致海鲜料理",
  },
  {
    tag: "甜点",
    title: "龙井奶冻与青梅露",
    copy: "茶香、奶香与青梅酸度平衡，给晚餐一个干净收束。",
    image: "https://images.unsplash.com/photo-1559847844-5315695dadae?auto=format&fit=crop&w=900&q=84",
    alt: "摆盘精致的甜点",
  },
];

const updateHeader = () => {
  isScrolled.value = window.scrollY > 20;
};

const closeMenu = () => {
  isMenuOpen.value = false;
};

const submitBooking = () => {
  const name = booking.name.trim() || "您好";
  formNote.value = `${name}，预订信息已收到，我们会尽快电话确认。`;
  booking.name = "";
  booking.phone = "";
  booking.partySize = "";
  booking.date = "";
};

onMounted(() => {
  updateHeader();
  window.addEventListener("scroll", updateHeader, { passive: true });
});

onBeforeUnmount(() => {
  window.removeEventListener("scroll", updateHeader);
});
</script>

<template>
  <header
    class="site-header"
    :class="{ 'is-scrolled': isScrolled, 'is-open': isMenuOpen }"
    aria-label="主导航"
  >
    <a class="brand" href="#top" aria-label="松间食社首页" @click="closeMenu">
      <span class="brand-mark">松</span>
      <span>松间食社</span>
    </a>
    <nav class="nav-links" aria-label="页面导航">
      <a href="#menu" @click="closeMenu">菜单</a>
      <a href="#space" @click="closeMenu">环境</a>
      <a href="#booking" @click="closeMenu">订座</a>
    </nav>
    <a class="header-action" href="#booking">立即预订</a>
    <button
      class="menu-toggle"
      type="button"
      aria-label="打开菜单"
      :aria-expanded="String(isMenuOpen)"
      @click="isMenuOpen = !isMenuOpen"
    >
      <span></span>
      <span></span>
    </button>
  </header>

  <main id="top">
    <section class="hero" aria-label="餐厅介绍">
      <picture>
        <source
          media="(max-width: 720px)"
          srcset="https://images.unsplash.com/photo-1551218808-94e220e084d2?auto=format&fit=crop&w=1100&q=85"
        />
        <img
          src="https://images.unsplash.com/photo-1414235077428-338989a2e8c0?auto=format&fit=crop&w=2200&q=88"
          alt="餐厅餐桌上摆放着精致菜品与酒杯"
        />
      </picture>
      <div class="hero-overlay"></div>
      <div class="hero-content">
        <p class="eyebrow">Modern Chinese Table</p>
        <h1>松间食社</h1>
        <p class="hero-copy">
          以江南时令入菜，把炭火、清汤与手作点心放进一顿从容晚餐。
        </p>
        <div class="hero-actions">
          <a class="primary-button" href="#booking">预订今晚</a>
          <a class="ghost-button" href="#menu">查看菜单</a>
        </div>
      </div>
      <aside class="hero-card" aria-label="今日营业信息">
        <span>今日营业</span>
        <strong>11:30 - 22:30</strong>
        <small>静安寺 · 愚园路 128 号</small>
      </aside>
    </section>

    <section class="intro section-band">
      <div>
        <p class="section-kicker">Seasonal Dining</p>
        <h2>一桌有烟火气，也有留白。</h2>
      </div>
      <p>
        我们每天依据市场鲜货微调菜单：清晨到货的河鲜、刚采下的蔬菜、慢熬八小时的汤底，
        配合开放式厨房里的炭火香气，做出轻盈但有记忆点的现代中式料理。
      </p>
    </section>

    <section class="menu-section" id="menu">
      <div class="section-heading">
        <p class="section-kicker">Signature Menu</p>
        <h2>招牌菜单</h2>
      </div>
      <div class="dish-grid">
        <article v-for="dish in dishes" :key="dish.title" class="dish-card">
          <img :src="dish.image" :alt="dish.alt" />
          <div>
            <span>{{ dish.tag }}</span>
            <h3>{{ dish.title }}</h3>
            <p>{{ dish.copy }}</p>
          </div>
        </article>
      </div>
    </section>

    <section class="space-section" id="space">
      <div class="space-image">
        <img
          src="https://images.unsplash.com/photo-1514933651103-005eec06c04b?auto=format&fit=crop&w=1600&q=86"
          alt="温暖灯光下的餐厅室内空间"
        />
      </div>
      <div class="space-copy">
        <p class="section-kicker">The Room</p>
        <h2>开放厨房、长桌、两间安静包厢。</h2>
        <p>
          午间适合商务简餐，夜晚适合朋友小聚和纪念日。座位之间保留足够距离，
          让谈话不用压低，也不会被旁桌打断。
        </p>
        <ul class="feature-list">
          <li><span>42</span>个大厅座位</li>
          <li><span>2</span>间独立包厢</li>
          <li><span>180+</span>款自然酒与清酒</li>
        </ul>
      </div>
    </section>

    <section class="booking-section" id="booking">
      <div class="booking-copy">
        <p class="section-kicker">Reservation</p>
        <h2>今晚留一个位置。</h2>
        <p>填写信息后，我们会在 15 分钟内电话确认座位与忌口。</p>
      </div>
      <form class="booking-form" @submit.prevent="submitBooking">
        <label>
          姓名
          <input v-model="booking.name" type="text" name="name" placeholder="例如：林小姐" required />
        </label>
        <label>
          电话
          <input v-model="booking.phone" type="tel" name="phone" placeholder="用于确认订座" required />
        </label>
        <label>
          人数
          <select v-model="booking.partySize" name="party-size" required>
            <option value="">请选择</option>
            <option>2 位</option>
            <option>3-4 位</option>
            <option>5-8 位</option>
            <option>包厢咨询</option>
          </select>
        </label>
        <label>
          日期
          <input v-model="booking.date" type="date" name="date" :min="today" required />
        </label>
        <button type="submit">提交预订</button>
        <p class="form-note" role="status" aria-live="polite">{{ formNote }}</p>
      </form>
    </section>
  </main>

  <footer class="site-footer">
    <p>松间食社 · 上海静安区愚园路 128 号</p>
    <p>021-6288-0916 · 每日 11:30 - 22:30</p>
  </footer>
</template>
