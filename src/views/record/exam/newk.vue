<template>
  <el-container style="height: 100vh; border: 1px solid #eee">
    <!-- <div class="left">
      <div class="fk">
        <div
          style="
            font-size: 25px;
            font-weight: 500;
            margin-left: 10px;
            padding: 10px 0 0 0;
          "
        >
          11
        </div>
        <div class="sj">
          <div>
            <span>得分</span>
            <span>50</span>
          </div>
          <div>
            <span>耗时</span>
            <span>50</span>
          </div>
          <div>
            <span>提交人</span>
            <span>50</span>
          </div>
          <el-divider></el-divider>
          <p>
            共 <span style="color: #1890ff">5 </span> 题, 共
            <span style="color: #1890ff">100</span> 分
          </p>
          <el-row>
            <el-tag
              v-for="index in 5"
              :type="index === quIndex ? 'success' : ''"
              @click="handleTag(index)"
              class="type_tag"
            >
              {{ index }}
            </el-tag>
          </el-row>
        </div>
      </div>
    </div> -->

    <el-container>
      <el-main class="right">
        <el-col>
          <el-card class="qu_list">
            <div>
              <!-- eslint-disable-next-line vue/no-template-shadow -->
              <template v-for="(index, indexx) in data">
                <!-- eslint-disable-next-line vue/require-v-for-key -->
                <div
                  v-if="
                    index.quType === 1 ||
                    index.quType === 2 ||
                    index.quType === 3
                  "
                  :class="'index' + index"
                >
                  <el-row :gutter="24">
                    <el-col :span="20" style="text-align: left">
                      <!-- 题目: 序号、类型、题干 -->
                      <div>
                        <!-- <div class="qu_num">{{ index }}</div> -->
                        <!-- 【 单选题 】 -->
                        <div class="qu_content">
                          {{ indexx + 1 }}、{{ index.title }}
                        </div>

                        <!-- <div v-if="item.image != null && item.image != ''" style="clear: both">
                          <el-image :src="item.image" style="max-width: 200px" />
                        </div> -->
                      </div>
                      <div v-if="index.image != null && index.image != ''">
                        <el-image :src="index.image" 
                        :preview-src="[index.image]" 
                         style="height: 100px" />
                      </div>
                      <!-- 选项 -->
                      <el-radio-group class="qu_choose_group">
                        <!-- ['A', 'B', 'C', 'D'] -->
                        <el-radio
                          v-for="(item, indexs) in index.option"
                          :key="indexs"
                          :label="item.content"
                          border
                          class="qu_choose"
                          :class="{
                            imgC: item.image != null && item.image != '',
                            isRight:
                              index.myOption != null &&
                              isCheck(index.myOption, item.sort) &&
                              item.isRight,
                            incorrect:
                              index.myOption != null &&
                              isCheck(index.myOption, item.sort) &&
                              !item.isRight,
                          }"
                        >
                          <!-- 选项flex浮动 -->
                          <div class="qu_choose_tag">
                            <div class="qu_choose_tag_type">
                              {{ numberToLetter(indexs) }}、{{ item.content }}.
                            </div>
                            <!-- 选项内容和图片 -->
                            <div
                              v-if="item.image != null && item.image != ''"
                              style="clear: both"
                            >
                              <el-image
                                :src="item.image"
                                :preview-src="[item.image]" 
                                style="max-width: 200px"
                              />
                            </div>
                            <div v-if="item.image != null && item.image != ''">
                              <el-image
                                :src="item.image"
                                :preview-src="[item.image]" 
                                class="qu_choose_tag_img"
                              />
                            </div>
                          </div>
                        </el-radio>
                      </el-radio-group>

                      <!-- 题目解析 -->
                      <div class="qu_analysis">
                        <el-card>
                          <div>
                            <span>考生答案：</span>
                            <span>{{ numberToLetter(index.myOption) }}</span
                            ><br />
                          </div>
                          <div style="margin-top: 8px">
                            <span>正确答案：</span>
                            <span>{{ numberToLetter(index.rightOption) }}</span
                            ><br />
                          </div>
                          <div style="margin-top: 8px">
                            <span>试题解析：</span>
                            <span>{{ index.analyse }}</span
                            ><br />
                          </div>
                        </el-card>
                      </div>
                    </el-col>
                  </el-row>
                  <el-divider />
                </div>
              </template>
              <!-- eslint-disable-next-line vue/no-template-shadow -->
              <template v-for="index in data">
                <!-- eslint-disable-next-line vue/require-v-for-key -->
                <div v-if="index.quType === 4" :class="'index' + index">
                  <el-row :gutter="24">
                    <el-col :span="20" style="text-align: left">
                      <!-- 题目: 序号、类型、题干 -->
                      <div>
                        <!-- <div class="qu_num">{{ index }}</div> -->
                        <!-- 【 单选题 】 -->
                        <div class="qu_content">{{ index.title }}</div>
                      </div>

                      <!-- 选项 -->
                      <el-radio-group class="qu_choose_group">
                        <!-- ['A', 'B', 'C', 'D'] -->
                        <el-input
                          v-model="index.myOption"
                          style="margin-top: 10px"
                          type="textarea"
                          :autosize="{ minRows: 2, maxRows: 4 }"
                          placeholder=""
                          :disabled="true"
                        />
                      </el-radio-group>

                      <!-- 题目解析 -->
                      <div class="qu_analysis">
                        <el-card>
                          <div>
                            <!-- <span>考生答案：</span>
                            <span
                              :style="{
                                color:
                                  isRight === 1
                                    ? 'green'
                                    : isRight === 0
                                    ? 'red'
                                    : 'gray',
                              }"
                              >{{}}</span
                            ><br /> -->
                          </div>
                          <div style="margin-top: 8px">
                            <span>正确答案：</span>
                            <span>{{ index.rightOption }}</span>
                            <br />
                          </div>
                          <div style="margin-top: 8px">
                            <span>试题解析：</span>
                            <span>{{ index.analyse }}</span
                            ><br />
                          </div>
                        </el-card>
                      </div>
                    </el-col>
                  </el-row>
                  <el-divider />
                </div>
              </template>
            </div>
            <el-divider />
          </el-card>
        </el-col>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import { recordExamDetail } from "@/api/record";
export default {
  name: "ExamProcess",
  data() {
    return {
      input: "",
      quIndex: -1,
      examId: 0,
      data: null,
      userId: null,
      index: {
        quType: 4, // 确保这里有一个值
      },
    };
  },
  created() {
    if (this.$route.query?.data?.type === 1) {
      this.userId = this.$route.query.data.userId;
    }
    // this.examId=this.$route.query.zhi.examId
    this.examId = localStorage.getItem("record_exam_examId");
    this.ExamDetail();
  },
  methods: {
    isCheck(myOption, sort) {
      const arr = myOption.split(",").map(Number); // 将字符串转换为数字数组
      if (arr.includes(sort)) {
        return true;
      } else {
        return false;
      }
    },
    numberToLetter(input) {
      const numberToCharMap = {
        0: "A",
        1: "B",
        2: "C",
        3: "D",
        4: "E",
        5: "F",
      };

      // 辅助函数：将单个数字（字符串或数字类型）转换为字母
      const singleNumberToLetter = (num) =>
        numberToCharMap[parseInt(num, 10)] || "";

      // 辅助函数：处理逗号分隔的数字字符串
      const commaSeparatedNumbersToLetters = (str) => {
        const numbers = str.split(",").map((item) => parseInt(item.trim(), 10));
        return numbers.map((number) => numberToCharMap[number] || "").join(",");
      };

      // 判断输入类型并调用相应函数
      if (/^\d+$/.test(input)) {
        // 单个数字（字符串形式也可以匹配）
        return singleNumberToLetter(input);
      } else if (/^\d+(,\d+)*$/.test(input)) {
        // 包含逗号分隔的数字字符串
        return commaSeparatedNumbersToLetters(input);
      } else {
        return ""; // 输入不符合预期，返回空字符串或根据需要处理
      }
    },
    // 分页查询
    async ExamDetail() {
      const params = { examId: this.examId, userId: this.userId };
      const res = await recordExamDetail(params);
      this.data = res.data;
    },
    // 点击答题卡题号, 右侧题目滑动
    handleTag(index) {
      // 高亮选中的题目index标签
      this.quIndex = index;
      // 题目滑动到锚定点
      const page = document.querySelector(".index" + index);
      page.scrollIntoView();
    },
  },
};
</script>

<style scoped lang="scss">
.content {
  width: 97%;
  height: 60px;
  border: 1px solid #0a84ff;
  margin-top: 8px;
  margin-left: 10px;
  padding: 10px;
  font-weight: 200;
}
.sj {
  margin-top: 10px;
  margin-left: 10px;
  line-height: 22px;
}
.isRight {
  background-color: rgb(215, 245, 215);
}
.incorrect {
  background-color: rgb(248, 197, 197);
}
.fk {
  width: 200px;
  height: 100%;
  box-shadow: 0 0 15px rgb(197, 197, 197);
  margin: auto;
  margin-top: 20px;
  margin-left: 15px;
}
.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;
}

.left {
  width: 250px;
  height: 100%;
}
.right {
  width: 70%;
  height: 100%;
}
.el-divider--horizontal {
  display: block;
  height: 1px;
  width: 95%;
  margin: 24px 0;
}
.type_tag {
  margin-right: 5px;
  margin-top: 10px;
}

// 试题内容样式
.qu_list {
  height: 100%;
  width: 100%;
  overflow: auto;
  page-break-after: always;

  .qu_num {
    display: inline-block;
    // background: url('~@/assets/images/tkxl/btbj.png') no-repeat 100% 100%;
    background-size: contain;
    height: 30px;
    width: 30px;
    line-height: 25px;
    color: #fff;
    font-size: 14px;
    text-align: center;
    margin-right: 15px;
    flex-shrink: 0;
  }

  .qu_content {
    padding-left: 10px;
  }

  // 选项组
  .qu_choose_group {
    width: 100%;

    // 单个选项
    .qu_choose {
      display: block;
      margin: 10px;

      // 去除前面的radio
      ::v-deep .el-radio__input .el-radio__inner {
        display: none;
      }

      // 单个选项内容样式
      .qu_choose_tag {
        display: inline-flex;
        width: 90%;
        // 选项标签
        .qu_choose_tag_type {
          font-weight: bold;
          // color: #0a84ff;
          width: 10px;
        }
        // 选项内容
        .qu_choose_tag_content {
          padding: 0 10px 10px 10px;
        }
        .qu_choose_tag_img {
          // max-height:90px;
          // max-width:300px;
          height: 100px;
          display: block;
          margin: 10px;
        }

        .qu_choose_tag_el_image {
          clear: both;
          padding-top: 10px;
        }
      }
      // 选项答案
      .qu_choose_answer {
        float: right;
      }
    }
  }

  // 试题解析
  .qu_analysis {
    padding: 10px;

    .qu_analysis_content {
      padding-top: 10px;
    }
  }

  // 试题赋分
  .qu_assign_score {
    background: #f5f5f5;
    height: 100px;
    padding-top: 35px;

    .qu_assign_score_content {
      width: 80px;
    }
  }
}
.imgC {
  height: 150px;
}
</style>
