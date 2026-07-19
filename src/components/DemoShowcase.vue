<template>
  <section class="glass-card showcase-container">
    <div class="showcase-header">
      <div class="showcase-title-group">
        <h2 class="showcase-title">科学胎教与胎动音律案例库 (30 精选样例)</h2>
        <p class="showcase-subtitle">体验不同孕周阶段的莫扎特音律、爸爸语言故事、抚触律动与静心冥想，点击“一键同款生成”即可即刻核算</p>
      </div>
      <div class="showcase-badge">科学胎教 · 免费体验</div>
    </div>

    <!-- 搜索与分类筛选 -->
    <div class="showcase-filter-bar">
      <div class="category-tabs">
        <button 
          v-for="cat in categories" 
          :key="cat"
          class="category-tab"
          :class="{ active: currentCategory === cat }"
          @click="currentCategory = cat"
        >
          {{ cat }}
        </button>
      </div>
      <div class="search-input-wrapper">
        <input 
          v-model="searchQuery"
          type="text"
          placeholder="搜索孕周阶段、胎教音乐、爸爸对话、抚触、流派或关键字..."
          class="search-input"
        />
      </div>
    </div>

    <!-- 样例列表格 Grid -->
    <div class="sample-grid">
      <div 
        v-for="sample in paginatedSamples" 
        :key="sample.id" 
        class="sample-card"
      >
        <div class="sample-card-header">
          <span class="topic-category-tag">{{ sample.category }}</span>
          <span class="style-name-tag">{{ sample.style }}</span>
        </div>
        <div class="sample-original">
          <span class="sample-label">胎教阶段：</span>“{{ sample.destination }}，诉求：{{ sample.topic }}”
        </div>
        <div class="sample-rewritten">
          <span class="sample-label">胎教指南：</span>{{ sample.core }}
        </div>
        <div class="sample-card-footer">
          <button class="use-sample-btn" @click="$emit('use-sample', sample.topic, sample.destination)">
            一键同款生成
          </button>
        </div>
      </div>
    </div>

    <!-- 空状态提示 -->
    <div v-if="filteredSamples.length === 0" class="empty-showcase">
      未找到匹配的科学胎教案例，请尝试切换分类或重置搜索关键词。
    </div>

    <!-- 分页组件 -->
    <div v-if="filteredSamples.length > pageSize" class="pagination-bar">
      <button 
        class="page-btn" 
        :disabled="currentPage === 1"
        @click="currentPage--"
      >
        上一页
      </button>
      <span class="page-info">第 {{ currentPage }} / {{ totalPages }} 页 (共 {{ filteredSamples.length }} 条)</span>
      <button 
        class="page-btn" 
        :disabled="currentPage === totalPages"
        @click="currentPage++"
      >
        下一页
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

defineEmits<{
  (e: 'use-sample', text: string, destination: string): void;
}>();

const categories = ['全部', '音乐胎教', '语言对话', '抚触互动', '孕妇冥想', '爸爸参与'];
const currentCategory = ref('全部');
const searchQuery = ref('');
const currentPage = ref(1);
const pageSize = 6;

interface TaijiaoSample {
  id: number;
  category: string;
  destination: string;
  topic: string;
  style: string;
  core: string;
}

// 精选 30 条科学胎教案例
const raw30Samples: TaijiaoSample[] = [
  {
    id: 1,
    category: '音乐胎教',
    destination: '孕期24周 (听觉发育敏感期)',
    topic: '求适合晚上9点聆听的古典音乐清单，关注分贝与耳塞距离安全。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：选用莫扎特《D大调双钢琴奏鸣曲 K.448》，音量控制在50-60分贝，播放源距离腹部1米以外，每次15分钟。'
  },
  {
    id: 2,
    category: '爸爸参与',
    destination: '孕期28周 (胎动活跃期)',
    topic: '爸爸每天下班回家想和胎宝说话，求专属于爸爸的中频对话脚本。',
    style: '爸爸对话与亲子语言流',
    core: '指南：爸爸用低沉平缓的中频声音轻贴腹部：“小宝贝，我是爸爸，今天外面天气晴朗，爸爸今天给你讲小熊过河的故事...”'
  },
  {
    id: 3,
    category: '抚触互动',
    destination: '孕期20周 (胎动初现)',
    topic: '刚感觉到轻微胎动，求安全腹部抚触手法与推压节奏。',
    style: '抚触律动与游戏互动流',
    core: '指南：孕妇双手由上至下、由左至右轻轻掌心抚摸腹部，配合微弱胎动回应，每次5-10分钟，忌剧烈揉搓。'
  },
  {
    id: 4,
    category: '孕妇冥想',
    destination: '孕期32周 (孕晚期睡眠焦虑)',
    topic: '孕妇夜间容易惊醒紧张，求睡前自然声响与呼吸冥想导引。',
    style: '自然声响与静心冥想流',
    core: '指南：播放海浪与雨声白噪音，伴随腹式呼吸（吸气4秒，呼气6秒），感受全身肌肉放松，引导胎宝一同入睡。'
  },
  {
    id: 5,
    category: '语言对话',
    destination: '孕期16周 (听觉萌芽期)',
    topic: '开始进行早期语言输入，求适合孕妈妈朗诵的经典古诗词。',
    style: '艺术审美与早启智育流',
    core: '指南：温和朗诵贺知章《咏柳》与孟浩然《春晓》，语调抑扬顿挫富有韵律感，让胎宝感受语言节奏之美。'
  },
  {
    id: 6,
    category: '音乐胎教',
    destination: '孕期18周 (孕中期启动)',
    topic: '求晨间清醒时聆听的高活泼度巴洛克音乐建议。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：推荐维瓦尔第《四季·春》，乐曲欢快优雅，帮助唤醒孕妇与胎宝晨间活力。'
  },
  {
    id: 7,
    category: '爸爸参与',
    destination: '孕期30周 (爸爸互动探视)',
    topic: '爸爸给胎宝讲睡前绘本故事，求情景对话设计。',
    style: '爸爸对话与亲子语言流',
    core: '指南：选择《猜猜我有多爱你》绘本，爸爸模仿小栗色陆地兔口吻，用温暖嗓音向胎宝表达爱意。'
  },
  {
    id: 8,
    category: '抚触互动',
    destination: '孕期26周 (踢肚反应明显)',
    topic: '胎宝在肚子里踢来踢去，求“弹钢琴”呼应互动游戏。',
    style: '抚触律动与游戏互动流',
    core: '指南：当胎宝踢右侧时，妈妈用食指轻轻点按右侧，稍停片刻观察胎宝是否再次呼应踢击。'
  },
  {
    id: 9,
    category: '孕妇冥想',
    destination: '孕期12周 (孕早期止吐)',
    topic: '孕吐严重心情烦躁，求舒缓情绪音乐与芳香瑜伽。',
    style: '自然声响与静心冥想流',
    core: '指南：听森林鸟鸣水流声，双手合十置于胸前进行浅呼吸，想象宝宝在子宫温水中安详漂浮。'
  },
  {
    id: 10,
    category: '语言对话',
    destination: '孕期22周 (语言丰富期)',
    topic: '日常生活中与胎宝实时交流（如做饭、遛弯时）。',
    style: '爸爸对话与亲子语言流',
    core: '指南：随时随地把身边场景讲给胎宝听（如“宝贝，妈妈现在在花园里，公园里的玫瑰花开得很红很香”）。'
  },
  {
    id: 11,
    category: '音乐胎教',
    destination: '孕期34周 (听觉成熟期)',
    topic: '求适合安抚频繁胎动的低频大提琴与钢琴曲。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：精选巴赫《G弦上的咏叹调》，低沉悠扬的大提琴旋律能有效稳定胎宝胎动节律。'
  },
  {
    id: 12,
    category: '爸爸参与',
    destination: '孕期36周 (临产关怀期)',
    topic: '爸爸给胎宝做产前心理安抚与安全感建立。',
    style: '爸爸对话与亲子语言流',
    core: '指南：爸爸抚摸孕肚：“宝贝别害怕，爸爸妈妈已经帮你准备好了漂亮衣服和小床，期待很快见到你。”'
  },
  {
    id: 13,
    category: '抚触互动',
    destination: '孕期24周 (光照胎教启动)',
    topic: '求科学光照胎教时间、手电筒安全亮度与位置。',
    style: '抚触律动与游戏互动流',
    core: '指南：使用弱光手电筒隔着衣服贴近肚皮照射，开灭各2秒，反复5次，训练胎宝视觉感知力，忌强光直射。'
  },
  {
    id: 14,
    category: '孕妇冥想',
    destination: '孕期28周 (孕中期放松)',
    topic: '求下午茶时间的音乐冥想与正念放松。',
    style: '自然声响与静心冥想流',
    core: '指南：搭配轻悠班得瑞《雪化之时》，孕妇闭目养神，让心情沉淀，将喜悦与安详能量传递给胎宝。'
  },
  {
    id: 15,
    category: '语言对话',
    destination: '孕期14周 (早期情感建构)',
    topic: '求孕妈妈每日定时的“晚安问候语”范例。',
    style: '爸爸对话与亲子语言流',
    core: '指南：“小宝贝，今天的一天结束啦，妈妈爱你，我们现在要闭上眼睛睡个好觉咯。”'
  },
  {
    id: 16,
    category: '音乐胎教',
    destination: '孕期20周 (音律习惯养成)',
    topic: '每天定时播放胎教音乐，求建立固定音乐记忆。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：固定在每日中午12点与晚上8点播放莫扎特《摇篮曲》，帮助胎宝建立起日夜作息记忆。'
  },
  {
    id: 17,
    category: '爸爸参与',
    destination: '孕期22周 (爸爸歌声胎教)',
    topic: '爸爸五音不全，求简单温馨的爸爸哼唱曲目。',
    style: '爸爸对话与亲子语言流',
    core: '指南：爸爸无需高超技巧，只需用真诚平稳的中音哼唱《小星星》或《虫儿飞》，声音越熟稔胎宝越安心。'
  },
  {
    id: 18,
    category: '抚触互动',
    destination: '孕期30周 (肢体推抚)',
    topic: '感觉胎宝在肚子一侧鼓起小包，求安全推抚手法。',
    style: '抚触律动与游戏互动流',
    core: '指南：确定无宫缩的前提下，用掌心温和推抚凸起部位，若感到抵抗应立即停止，切忌强行用力。'
  },
  {
    id: 19,
    category: '孕妇冥想',
    destination: '孕期38周 (产前备战)',
    topic: '临产焦虑分娩恐惧，求拉玛泽呼吸配套冥想音乐。',
    style: '自然声响与静心冥想流',
    core: '指南：播放颂钵与阿尔法波助眠乐，练习宫缩间歇的慢胸式呼吸，建立顺利自然分娩的信念。'
  },
  {
    id: 20,
    category: '语言对话',
    destination: '孕期25周 (中英文双语感知)',
    topic: '进行简单的双语英语童谣朗读（如Twinkle Twinkle）。',
    style: '艺术审美与早启智育流',
    core: '指南：用清晰缓慢的语调朗读英文简短诗歌，重点在于母语般柔和的韵律节奏而非硬性记忆。'
  },
  {
    id: 21,
    category: '音乐胎教',
    destination: '孕期15周 (早期音乐感知)',
    topic: '适合孕妇边做家务/工作边背景播放的纯音乐。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：选海顿《水上音乐》或德彪西《月光》，舒缓优雅的背景音乐能维持孕妇血清素处于高位。'
  },
  {
    id: 22,
    category: '爸爸参与',
    destination: '孕期27周 (爸爸陪伴讲古诗)',
    topic: '爸爸喜欢国学，求适合讲给胎宝听的唐诗宋词。',
    style: '爸爸对话与亲子语言流',
    core: '指南：爸爸朗诵《小池》“泉眼无声惜细流，树阴照水爱晴柔”，向胎宝描绘夏日荷花生机盎然的景象。'
  },
  {
    id: 23,
    category: '抚触互动',
    destination: '孕期18周 (肚皮润肤抚触)',
    topic: '涂抹防妊娠纹橄榄油时的同步胎教抚触。',
    style: '抚触律动与游戏互动流',
    core: '指南：双手蘸取温热橄榄油，顺时针方向在腹部打圈轻揉，边抚摸边与胎宝低语表达爱意。'
  },
  {
    id: 24,
    category: '孕妇冥想',
    destination: '孕期21周 (午间放松)',
    topic: '午休20分钟快速恢复精力的胎教冥想法。',
    style: '自然声响与静心冥想流',
    core: '指南：聆听竹林风声与磬音，引导面部肌肉放松，想象阳光洒满腹部给胎宝带来温暖。'
  },
  {
    id: 25,
    category: '语言对话',
    destination: '孕期31周 (家庭成员大合唱)',
    topic: '大宝（哥哥/姐姐）一起参与给肚子里的小宝宝讲故事。',
    style: '爸爸对话与亲子语言流',
    core: '指南：鼓励大宝摸着妈妈肚皮说：“小妹妹/小弟弟，我是哥哥，等你出来我把最喜欢的玩具车分给你玩。”'
  },
  {
    id: 26,
    category: '音乐胎教',
    destination: '孕期29周 (胎律共振)',
    topic: '求中国传统乐器（古筝、琵琶、笛子）胎教曲目。',
    style: '莫扎特音乐与音律熏陶流',
    core: '指南：推荐古筝曲《高山流水》与笛子曲《姑苏行》，意境高远曲调优雅，非常适合文化熏陶。'
  },
  {
    id: 27,
    category: '爸爸参与',
    destination: '孕期19周 (爸爸周末胎教游戏)',
    topic: '周末爸爸陪伴孕妇进行一小时全方位亲子胎教。',
    style: '爸爸对话与亲子语言流',
    core: '指南：流程安排：10分钟爸爸轻音乐准备 -> 15分钟爸爸绘本讲读 -> 10分钟温和抚触 -> 孕妇安心休息。'
  },
  {
    id: 28,
    category: '抚触互动',
    destination: '孕期33周 (胎动应答)',
    topic: '观察胎宝对不同位置触碰的喜好与应答。',
    style: '抚触律动与游戏互动流',
    core: '指南：建立“触碰-停顿-观察”三步反馈法，记录胎宝最活跃的呼应时段，作为未来新生儿抚触习惯储备。'
  },
  {
    id: 29,
    category: '孕妇冥想',
    destination: '孕期23周 (孕妇悦己画画静息)',
    topic: '孕妇画画写字时的静态胎教。',
    style: '艺术审美与早启智育流',
    core: '指南：伴随舒缓爵士或莫扎特音乐，孕妇专注进行数字油画或水彩创作，将专注宁静的心境共享给胎宝。'
  },
  {
    id: 30,
    category: '语言对话',
    destination: '孕期35周 (胎宝命名互动)',
    topic: '孕妈妈和爸爸一起在腹旁讨论胎宝小名与美好寓意。',
    style: '爸爸对话与亲子语言流',
    core: '指南：父母交替叫唤拟定的小名（如“小暖暖”或“安安”），用温柔呼唤建立出生前的名字记忆。'
  }
];

const samples = ref<TaijiaoSample[]>(raw30Samples);

const filteredSamples = computed(() => {
  return samples.value.filter(s => {
    const matchCat = currentCategory.value === '全部' || s.category === currentCategory.value;
    const matchQuery = !searchQuery.value.trim() || 
      s.topic.includes(searchQuery.value) || 
      s.destination.includes(searchQuery.value) ||
      s.style.includes(searchQuery.value) || 
      s.core.includes(searchQuery.value);
    return matchCat && matchQuery;
  });
});

const totalPages = computed(() => Math.ceil(filteredSamples.value.length / pageSize) || 1);

const paginatedSamples = computed(() => {
  const start = (currentPage.value - 1) * pageSize;
  return filteredSamples.value.slice(start, start + pageSize);
});

watch([currentCategory, searchQuery], () => {
  currentPage.value = 1;
});
</script>
