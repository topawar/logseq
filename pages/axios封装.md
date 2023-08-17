- 引入axios#axios
- 引入qs
	- ```
	  yarn add axios	
	  yarn add Qs
	  ```
- 简单封装，详细配置参考官网 [axios文档](https://axios-http.com/zh/docs/intro)
- [[$red]]==qs引用名不要用大写的Qs,会有坑 `import qs from "qs";`==
- id:: 6496b345-2aa2-4752-a0ea-7fadf9299e00
  ``` typescript
  import axios, {AxiosError, AxiosInstance, AxiosRequestConfig, AxiosResponse} from "axios";
  import qs from "qs"
  
  const service: AxiosInstance = axios.create({
      baseURL: '/api',
      timeout: 50000,
      responseEncoding: "UTF-8",
      responseType: "json",
  })
  
  /**
   * 请求拦截器
   */
  // @ts-ignore
  service.interceptors.request.use((config: AxiosRequestConfig) => {
      config.transformRequest = [function (data) {
          // @ts-ignore
          return Qs.stringify(data)
      }]
      // @ts-ignore
      config.headers.set("tokenId",window.localStorage.getItem("tokenId"))
      return config;
  }, (error: AxiosError) => {
      return Promise.reject(error)
  })
  
  
  // @ts-ignore
  service.interceptors.response.use((response: AxiosResponse) => {
      const {code, data, msg} = response.data
      return data;
  }, (error: AxiosError) => {
      return Promise.reject(error)
  })
  
  /* 导出封装的请求方法 */
  export const http = {
      get<T = any>(url: string, config?: AxiosRequestConfig): Promise<T> {
          return service.get(url, config)
      },
  
      post<T = any>(url: string, data?: object, config?: AxiosRequestConfig): Promise<T> {
          return service.post(url, data, config)
      },
  
      postAny<T = any>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
          return service.post(url, data, config)
      },
  
      put<T = any>(url: string, data?: object, config?: AxiosRequestConfig): Promise<T> {
          return service.put(url, data, config)
      },
  
      delete<T = any>(url: string, config?: AxiosRequestConfig): Promise<T> {
          return service.delete(url, config)
      }
  }
  ```
-
- 第二种封装
- ```ts
  import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios';
  
  interface IResponseData<T = any> {
    code: number;
    message: string;
    data: T;
  }
  
  export class HttpClient {
    private instance: AxiosInstance;
  
    constructor(baseURL: string) {
      this.instance = axios.create({
        baseURL,
        timeout: 5000,
        headers: {
          'Content-Type': 'application/json;charset=UTF-8',
        },
      });
  
      // 添加请求拦截器
      this.instance.interceptors.request.use((config) => {
        // 在发送请求之前做些什么
        console.log('请求拦截器：', config);
        return config;
      }, (error) => {
        // 对请求错误做些什么
        console.error('请求拦截器发生错误：', error);
        return Promise.reject(error);
      });
  
      // 添加响应拦截器
      this.instance.interceptors.response.use((response) => {
        // 对响应数据做些什么
        const responseData: IResponseData = response.data;
        console.log('响应拦截器：', responseData);
        return responseData.data;
      }, (error) => {
        // 对响应错误做些什么
        console.error('响应拦截器发生错误：', error);
        return Promise.reject(error);
      });
    }
  
    public async get<T>(url: string, config?: AxiosRequestConfig): Promise<T> {
      const response: AxiosResponse<IResponseData<T>> = await this.instance.get(url, config);
      return response.data.data;
    }
  
    public async post<T>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
      const response: AxiosResponse<IResponseData<T>> = await this.instance.post(url, data, config);
      return response.data.data;
    }
  }
  ```