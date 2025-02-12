1. 请通过我输入的接口信息，仿照以下demo生成api接口代码
2. 接口信息中的出入参数需要生成typescript类型。如果没有指定response类型，就根据接口信息中 response里example里的raw_parameter里的信息，生成接口response的ts类型，否则就用any表示resonse返回类型。
3. 请在接口代码中提供比较完善的注释
4. 入参只传入一个object对象data，对data对象提供ts类型声明
5. 出入参数中的page、limit、total等字段类型必须为数字
```
import request from "@mop/utils/request";
const api = import.meta.env.VITE_APP_BASE_API + "/admin";
const PATH = api + "/dimensionItem";
/** 新增主题活动 */
export function listGradeDimension(data: any) {
  return request({
    url: `${PATH}/listGradeDimension`,
    method: "post",
    data: data,
    loading: false,
  });
}
```
