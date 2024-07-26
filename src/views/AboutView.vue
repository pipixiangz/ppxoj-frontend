<template>
  <div class="extended-about-page">
    <div class="top-section">
      <div class="image-container">
        <img
          src="@/assets/judge5.png"
          alt="Featured image"
          class="featured-image"
        />
      </div>
      <div class="content-container">
        <div class="content-wrapper">
          <div class="tag">{{ tag }}</div>
          <h1 class="title">{{ title }}</h1>
          <p class="description">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="bottom-section">
      <div class="full-width-content">
        <h2>{{ additionalTitle }}</h2>
        <p>{{ additionalContent }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ExtendedAboutPage",
  data() {
    return {
      tag: "皮皮码来判判",
      title: "Write, Submit, Succeed!",
      description:
        "基于Vue 3 + Spring Boot + 微服务+ Docker的编程题目在线评测系统",
      additionalTitle: "More About My Project",
      additionalContent:
        "An online programming problem judge system (abbreviated as OJ) based on Vue 3 + Spring Boot + Spring Cloud microservices + Docker.\n" +
        "\n" +
        "On the system frontend, administrators can create and manage problems; users can freely search for problems, read them, write, and submit code.\n" +
        "\n" +
        "On the system backend, it can compile, run, and judge the correctness of the output in a code sandbox based on the test cases set by the administrators.\n" +
        "\n" +
        "The code sandbox can serve as an independent service, available for use by other developers.\n",
    };
  },
};
</script>

<style scoped>
.extended-about-page {
  background-color: #1a1a1a;
  color: white;
  font-family: Arial, sans-serif;
  width: 100%;
  overflow-x: hidden;
}

.top-section {
  display: flex;
  min-height: 80vh;
}

.image-container {
  flex: 6;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  padding: 2vw;
  box-sizing: border-box;
  border-radius: 20px; /* 添加圆角到容器 */
}

.featured-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  border-radius: 25px; /* 添加圆角到图片 */
}

.content-container {
  flex: 4;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2vw;
  box-sizing: border-box;
}

.content-wrapper {
  max-width: 100%;
  word-wrap: break-word;
  overflow-wrap: break-word;
}

.tag {
  font-size: clamp(12px, 1vw, 18px);
  margin-bottom: 1vh;
}

.title {
  font-size: clamp(24px, 2.5vw, 40px);
  font-weight: bold;
  margin-bottom: 2vh;
}

.description {
  font-size: clamp(16px, 1.2vw, 20px);
  line-height: 1.5;
}

.bottom-section {
  padding: 4vw;
}

.full-width-content {
  max-width: 800px;
  margin: 0 auto;
}

.full-width-content h2 {
  font-size: clamp(20px, 2.5vw, 36px);
  margin-bottom: 2vh;
}

.full-width-content p {
  font-size: clamp(16px, 1.2vw, 20px);
  line-height: 1.6;
}

@media (max-width: 768px) {
  .top-section {
    flex-direction: column;
    height: auto;
  }

  .image-container,
  .content-container {
    flex: none;
    width: 100%;
    max-width: 100%;
  }

  .image-container {
    min-height: 50vh;
  }

  .featured-image {
    border-radius: 15px; /* 在移动端稍微减小圆角 */
  }

  .content-container {
    padding: 4vw;
  }

  .tag {
    font-size: clamp(14px, 3vw, 20px);
  }

  .title {
    font-size: clamp(20px, 5vw, 32px);
  }

  .description {
    font-size: clamp(16px, 3.5vw, 22px);
  }

  .bottom-section {
    padding: 6vw;
  }
}
</style>
