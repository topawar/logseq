- 引入axios
- 引入qs
	- ```
	  yarn add axios	
	  yarn add Qs
	  ```
- 简单封装，详细配置参考官网 [axios文档](https://axios-http.com/zh/docs/intro)
- [[$red]]==qs引用名不要用大写的Qs,会有坑 `import qs from "qs";`==
- ``` typescript
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