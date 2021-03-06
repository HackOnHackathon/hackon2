<template>
  <Container id="sawoAndPolygon">
    <div class="contents">
      <div class="sponsoredPrizes">
        <div class="cardGrid">
          <div v-for="(prize, index) in sponsoredPrizes" :key="index">
            <div class="card">
              <img
                :class="['background', prize.name]"
                :src="prize.background"
                alt="background"
              />
              <div class="companyImage">
                <img :src="prize.logo" alt="logo" />
                <h4>{{ prize.name }}</h4>
              </div>
              <div class="details">
                <div class="inner">
                  <div class="card-side front">
                    <h4>
                      {{ prize.heading }}
                    </h4>
                    <p>
                      <span v-html="prize.details">{{}}</span>
                    </p>
                  </div>
                  <div class="card-side back">
                    <img v-if="prize.detailImage" :src="prize.detailImage" />
                    <span
                      v-for="(para, index) in prize.description"
                      :key="index"
                    >
                      <span v-html="para">{{}}</span>
                    </span>
                    <span class="learn-more" @click="showModal(prize.name)">
                      Learn More
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <PolygonModal v-show="isPolygonModalVisible" @close="closeModal()" />
    <SAWOModal v-show="isSAWOModalVisible" @close="closeModal()" />
  </Container>
</template>

<script>
import Container from "~/components/Container";
import SubHashHeader from "~/components/SubHashHeader";
import PolygonModal from "~/components/PolygonModal";
import SAWOModal from "~/components/SAWOModal";

export default {
  components: {
    Container,
    SubHashHeader,
    PolygonModal,
    SAWOModal,
  },
  data() {
    return {
      isPolygonModalVisible: false,
      isSAWOModalVisible: false,
      sponsoredPrizes: [
        {
          name: "SAWO Labs",
          detailImage: require("~/assets/Prizes/100-usd.svg"),
          background: require("~/assets/Prizes/sawoTransparent.png"),
          logo: require("~/assets/Prizes/sawo.png"),
          heading: "Best use of SAWO",
          details:
            "The best hack built using the SAWO Labs API",
          description: ["$100 Prize to winning team"],
        },
        {
          name: "Polygon",
          detailImage: require("~/assets/Prizes/5000.svg"),
          background: require("~/assets/Prizes/polygonTransparent.png"),
          logo: require("~/assets/Prizes/polygon.png"),
          heading: "Best Hack Built On Polygon",
          details:
            "The best hack built using Polygon Technology (previously Matic Network)",
          description: ["₹5000 Prize to winning team"],
          polygon: true,
        },
      ],
    };
  },
  methods: {
    showModal(prizeName) {
      if (prizeName === "Polygon") {
        this.isPolygonModalVisible = true;
        document.querySelector("body").style.overflow = "hidden";
      } else if (prizeName === "SAWO Labs") {
        this.isSAWOModalVisible = true;
        document.querySelector("body").style.overflow = "hidden";
      }
    },
    closeModal() {
      this.isPolygonModalVisible = false;
      this.isSAWOModalVisible = false;
      document.querySelector("body").style.overflow = "initial";
    },
  },
};
</script>

<style lang="scss" scoped>
.contents {
  padding: 20px 0;

  .sponsoredPrizes {
    @include respond-below(md) {
      grid-template-columns: repeat(3, 1fr);
      margin-top: 30px;
    }

    @include respond-below(sm) {
      grid-template-columns: repeat(2, 1fr);
      margin-top: 30px;
    }

    @include respond-below(xs) {
      grid-template-columns: repeat(1, 1fr);
      margin-top: 30px;
    }

    .cardGrid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      column-gap: 30px;
      row-gap: 30px;

      @include respond-below(md) {
        grid-template-columns: repeat(1, 1fr);
      }
      .card {
        background-color: var(--color-secondary-light) !important;
        position: relative;
        border-radius: 10px;
        display: flex;
        min-height: 350px;
        padding: 20px;
        overflow: hidden;

        @include respond-below(xs) {
          flex-direction: column;
          text-align: center;
        }

        .background {
          position: absolute;
          top: 50%;
          transform: translateY(-50%);

          &.SAWO,
          &.Polygon {
            height: 80%;
          }

          @include respond-below(xs) {
            display: none;
          }
        }

        .companyImage {
          text-align: center;
          position: absolute;
          top: 33%;
          left: 10%;

          @include respond-below(xs) {
            position: unset;
            margin-bottom: 20px;
          }

          @include respond-between(md, lg) {
            left: 3.5%;
            img {
              width: 100px;
            }
          }

          img {
            width: 120px;
            border-radius: 10px;
          }

          h4 {
            font-family: "Sen";
            font-size: 1.6rem;
            font-weight: 600;
          }
        }

        .details {
          position: absolute;
          min-height: calc(100% - 80px);
          text-align: center;
          border-radius: 10px;
          right: 30px;
          width: 56%;

          .inner {
            position: relative;
            min-height: 350px;
            @media (max-width: 580px) {
              width: 96%;
            }
            .card-side {
              width: 100%;
              border-radius: 15px;
              transition: all 0.8s ease;
              backface-visibility: hidden;
              position: absolute;
              display: flex;
              flex-direction: column;
              justify-content: center;
              padding: 0.5rem;
              min-height: 95%;
              color: white;
              background-color: var(--color-secondary);
            }
            .card-side.back {
              transform: rotateY(-180deg);
              .learn-more {
                cursor: pointer;
                color: #00ffa4;
              }
            }
            &:hover .card-side.front {
              transform: rotateY(180deg);
            }
            &:hover .card-side.back {
              transform: rotateY(0deg);
            }
          }

          @include respond-below(xs) {
            position: unset;
            width: unset;
            right: unset;
          }

          img {
            width: 70%;
            margin: 16px auto;
          }

          h4 {
            font-family: "Sen";
            font-size: 1.6rem;
            font-weight: 600;
          }

          p {
            margin-top: 20px;
            font-family: "Sen";
            font-size: 1.13rem;

            @include respond-between(md, lg) {
              font-size: 1rem;
            }
          }
        }
      }
    }
  }
}
</style>
