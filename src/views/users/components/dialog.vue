<template>
  <el-dialog
    :model-value="dialogVisible"
    :title="dialogTitle"
    width="40%"
    :before-close="handleClose"
  >
    <el-form ref="formRef" :model="form" label-width="70px" :rules="rules">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" />
      </el-form-item>
      <el-form-item
        label="密码"
        prop="password"
        v-if="dialogTitle === '添加用户'"
      >
        <el-input v-model="form.password" />
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="form.email" />
      </el-form-item>
      <el-form-item label="手机号" prop="mobile">
        <el-input v-model="form.mobile" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="handleClose">取消</el-button>
        <el-button type="primary" @click="handleConfirm(formRef)"
          >确认</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>

<script setup>
import { defineEmits, ref, defineProps, watch } from 'vue'
import { addUser, editUser } from '@/api/users'
import { useI18n } from 'vue-i18n'
import { ElMessage } from 'element-plus'
const i18n = useI18n()
const props = defineProps({
  dialogTitle: {
    type: String,
    default: '',
    required: true
  },
  dialogTableValue: {
    type: Object,
    default: () => {}
  }
})

const formRef = ref(null)
const form = ref({
  username: '',
  password: '',
  email: '',
  mobile: ''
})
const rules = ref({
  username: [
    { required: true, message: '输入正确用户名', trigger: 'blur' },
    { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '输入正确密码', trigger: 'blur' },
    { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' }
  ],
  mobile: [{ required: true, message: '输入正确手机号', trigger: 'blur' }],
  email: [
    {
      required: true,
      message: 'Please input email',
      trigger: 'blur'
    },
    {
      type: 'email',
      message: 'Please input correct email',
      trigger: ['blur', 'change']
    }
  ]
})

watch(
  () => props.dialogTableValue,
  () => {
    // console.log(props.dialogTableValue)
    form.value = props.dialogTableValue
  },
  { deep: true, immediate: true }
)

const emits = defineEmits(['update:modelValue', 'initUserList'])
const handleClose = () => {
  emits('update:modelValue', false)
}

const handleConfirm = (formEl) => {
  if (!formEl) return
  formEl.validate(async (valid, fields) => {
    if (valid) {
      props.dialogTitle === '添加用户'
        ? await addUser(form.value)
        : await editUser(form.value)
      ElMessage({
        message: i18n.t('message.updeteSuccess'),
        type: 'success'
      })
      emits('initUserList')
      handleClose()
    } else {
      console.log('error submit!', fields)
    }
  })
}
</script>

<style lang="scss" scoped></style>
