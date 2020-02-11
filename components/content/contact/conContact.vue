<template>
  <div class="conContent">
    <div class="content-wrape">
      <div class="left-side sec-50">
        <div class="inner-wrape title">
          <h5>INSTA TODOS</h5>
          <h6>h-works</h6>
          <!-- <a href="http://tourdehdr.sakuraweb.com/h-works-website/index.html">
              <h5>HIRAMATSU WORKS</h5>
            </a>
            <a href="http://tourdehdr.sakuraweb.com/h-works-website/index.html">
              <h5>Portfolio Site</h5>
            </a>
            <a href="http://tourdehdr.sakuraweb.com/h-works-website/index.html">
              <h6>
                http://tourdehdr.sakuraweb.com/h-works-website/index.html
              </h6>
            </a> -->
          <!-- <br />
            <p>
              このWEBサイトは、作品をご覧いただき、デザイン、コーディング、プログラミングなどのスキルレベルをご確認いただくために作成しております。
            </p> -->
        </div>
      </div>
      <div class="left-side sec-50">
        <div class="inner-wrape info">
          <h6>
            <i class="far fa-envelope" /><span>hiramatsu3300@gmail.com</span>
          </h6>
          <h6><i class="fas fa-phone" /><span>111-222-33333</span></h6>
          <br />
          <h6>Okayama</h6>
          <h6>Japan</h6>
        </div>
      </div>
      <!-- <div class="two-col">
        <div class="col-block left-block">
          <div class="section-block left-section">
            <div class="contact-title">
              <div class="link-text">
                <a
                  href="http://tourdehdr.sakuraweb.com/h-works-website/index.html"
                >
                  <h1>HIRAMATSU WORKS</h1>
                  <h5>Portfolio Site</h5>
                  <h5>
                    http://tourdehdr.sakuraweb.com/h-works-website/index.html
                  </h5>
                </a>
              </div>
              <br />
              <p>
                このWEBサイトは、作品をご覧いただき、デザイン、コーディング、プログラミングなどのスキルレベルをご確認いただくために作成しております。
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="two-col">
        <div class="col-block right-block">
          <div class="section-block left-section">
            <p>
              <i class="far fa-envelope" /><span>hiramatsu3300@gmail.com</span>
            </p>
            <p><i class="fas fa-phone" /><span>111-222-33333</span></p>
            <br />
            <p>Minami</p>
            <p>Okayama</p>
            <p>japan</p>
          </div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  //   props: {
  //     msg: String
  //   }
}
</script>

<style scoped lang="scss">
.conContent {
  position: relative;
  width: 100%;
  height: 100%;
  // height: calc(100vh - #{$header-height});
  padding: 0;
  margin: 0;
  @media (min-width: 992px) {
    padding: 8rem;
  }

  // @media (min-width: 768px) {
  //   margin: 8rem 8rem;
  // }
  // color: #fff;
  // a {
  //   color: #fff;
  //   text-decoration: none;
  //   cursor: pointer;
  //   :hover {
  //     opacity: 0.5;
  //   }
  // }
}
.content-wrape {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  @media (min-width: 992px) {
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
  }
  background-color: $footer-color-color;
}
.sec-50 {
  width: 100%;
  height: 50%;
  padding: 1rem;
  @media (min-width: 992px) {
    width: 50%;
    height: 100%;
  }
}

.inner-wrape {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.title,
.info {
  h5 {
    font-size: 2rem;
  }
  h6 {
    font-size: 1.2rem;
  }
  @media (min-width: 992px) {
    h5 {
      font-size: 3rem;
    }
    h6 {
      font-size: 1.6rem;
    }
  }
  @media (min-width: 1440px) {
    h5 {
      font-size: 4rem;
    }
    h6 {
      font-size: 2.2rem;
    }
  }
}
</style>
