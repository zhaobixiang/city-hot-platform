<template>
  <div v-show="visible" class="dialog">
    <div class="dialog-setting">
      <div class="dialog-setting-close" @click="hide"></div>
      <div class="dialog-setting-title">2103信息设置</div>
      <div class="dialog-setting-body">
        <div class="dialog-setting-body-item">
          <div class="dialog-setting-body-item-label">室温设置</div>
          <div class="dialog-setting-body-item-value">
            <el-input v-model="s1" style="width: 140px" />
          </div>
          <div class="dialog-setting-body-item-unit">(℃)</div>
        </div>
        <div class="dialog-setting-body-item">
          <div class="dialog-setting-body-item-label">回温设置</div>
          <div class="dialog-setting-body-item-value">
            <el-input v-model="s2" style="width: 140px" />
          </div>
          <div class="dialog-setting-body-item-unit">(℃)</div>
        </div>
        <div class="dialog-setting-body-item">
          <div class="dialog-setting-body-item-label">阀位设置</div>
          <div class="dialog-setting-body-item-value">
            <el-input v-model="s3" style="width: 140px" />
          </div>
          <div class="dialog-setting-body-item-unit">(%)</div>
        </div>
      </div>
      <div class="dialog-setting-footer" @click="save">保存</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const visible = ref('');
const s1 = ref('');
const s2 = ref('');
const s3 = ref('');

const show = (data = {}) => {
  // console.log(data);
  visible.value = true;
};

const hide = () => {
  visible.value = false;
};

const save = () => {
  console.log(s1.value, s2.value, s3.value);
  hide();
};

defineExpose({
  show,
  hide,
});
</script>

<style lang="scss" scoped>
.dialog {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;

  &-setting {
    position: relative;
    width: 290px;
    height: 310px;
    background: url('@/assets/dialog2.png') no-repeat;
    padding-left: 30px;

    &-close {
      position: absolute;
      top: 24px;
      right: 20px;
      width: 24px;
      height: 24px;
      background: url('@/assets/dialog-close.png') no-repeat;
      cursor: pointer;
    }

    &-title {
      padding-top: 20px;
      height: 40px;
      line-height: 40px;
      color: #01e3df;
      font-size: 14px;
      text-align: center;
      margin-left: -30px;
      letter-spacing: 2px;
    }

    &-body {
      padding: 20px 10px 0 24px;

      &-item {
        display: flex;
        align-items: center;
        color: #fff;
        margin-bottom: 16px;

        &:last-child {
          margin-bottom: 0;
        }

        &-label {
          width: 64px;
        }

        &-value {
          background: url('@/assets/input.png') no-repeat;
          margin: 0 6px 0 8px;

          :deep(.el-input__wrapper) {
            background-color: transparent;
            box-shadow: none;
            border-radius: 0;
          }
          :deep(.el-input__wrapper) {
            padding: 0 10px;
          }
          :deep(.el-input__inner) {
            height: 28px;
            line-height: 28px;
            color: #fff;
          }
        }
      }
    }

    &-footer {
      width: 222px;
      text-align: center;
      height: 60px;
      line-height: 48px;
      color: #fff;
      font-size: 16px;
      margin-top: 22px;
      margin-left: 32px;
      background: url('@/assets/button.png') no-repeat;
      cursor: pointer;
    }
  }
}
</style>
