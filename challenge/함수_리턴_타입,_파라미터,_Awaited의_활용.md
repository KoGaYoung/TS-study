## 문제

<img width="707" alt="image" src="https://github.com/KoGaYoung/TS-study/assets/36693355/37975f02-f8c8-4198-87d0-c40046c12b84">


## 풀이
~~~typescript
// GetLocationWeatherReturn, GetDetailedWeatherParameters 에 커서를 가져다대면 호출 시그니처를 알려준다
import { Equal, Expect } from '../../helper';

type LocationId = string;

const getLocationWeather = (locationId: LocationId) => {
  return `Weather at location ${locationId}`;
};
type GetLocationWeatherReturn = ReturnType<typeof getLocationWeather>;

const getDetailedWeather = (
  locationId: LocationId,
  details?: {
    tempUnit?: 'C' | 'F';
    includeForecast?: boolean;
  }
) => {};

type GetDetailedWeatherParameters = Parameters<typeof getDetailedWeather>;

type tests = [
  Expect<Equal<GetLocationWeatherReturn, string>>,
  Expect<
    Equal<
      GetDetailedWeatherParameters,
      [
        locationId: LocationId,
        details?:
          | {
              tempUnit?: 'C' | 'F' | undefined;
              includeForecast?: boolean | undefined;
            }
          | undefined
      ]
    >
  >
];

~~~
