# Explanation
## constness 나 volatility 를 제거하는 경우를 제외하고, `dynamic_cast`로는 다음과 같은 변환만 수행할 수 있다.

1. **타입 일치 또는 `const` 추가:** 만약 `expression`의 타입이 `target-type`과 정확히 일치하거나, 덜 `cv-qualified` (const와 volatile 한정어 제거) 버전이라면, 결과는 `expression`의 값이며, 타입은 `target-type`입니다. (`dynamic_cast`는 `const`를 추가하는데 사용될 수 있습니다. 이 변환은 암시적 변환과 `static_cast`로도 수행할 수 있습니다.)
2. **널 포인터 값:** `expression`의 값이 널 포인터 값인 경우, 결과는 `target-type`의 널 포인터 값입니다.
3. **베이스 클래스에서 파생 클래스로의 변환:** `target-type`이 `Base` 클래스의 포인터나 참조이고, `expression`의 타입이 `Derived` 클래스의 포인터나 참조일 때, 여기서 `Base`는 `Derived`의 고유하고 접근 가능한 베이스 클래스라면, 결과는 `Derived` 객체 내의 `Base` 클래스 하위 객체를 가리키는 포인터나 참조입니다. (이 변환은 암시적 변환과 `static_cast`로도 수행할 수 있습니다.)
4. **void 포인터로의 변환:** `expression`이 다형성 타입의 포인터이고, `target-type`이 `void` 포인터라면, 결과는 `expression`이 가리키거나 참조하는 가장 파생된 객체의 포인터입니다.
5. **다운캐스트 및 사이드캐스트:**
	- **다운캐스트:** `expression`이 다형성 타입 `Base`의 포인터나 참조이고, `target-type`이 `Derived` 타입의 포인터나 참조일 경우, 런타임에서 확인이 수행됩니다. `expression`이 가리키거나 참조하는 객체가 `Derived`의 공개 베이스이고, 해당 베이스에서 파생된 `Derived` 타입 객체가 하나만 존재한다면, 캐스트 결과는 그 `Derived` 객체를 가리키거나 참조합니다.
	- **사이드캐스트:** 그렇지 않고 `expression`이 가장 파생된 객체의 공개 베이스를 가리키거나 참조하며, 동시에 가장 파생된 객체가 `Derived` 타입의 모호하지 않은 공개 베이스 클래스를 가지고 있다면, 캐스트 결과는 그 `Derived`를 가리키거나 참조합니다.
	- **캐스트 실패:** 그 외의 경우에는 런타임 확인이 실패합니다. `dynamic_cast`가 포인터에 사용된 경우 `target-type`의 널 포인터 값이 반환됩니다. 참조에 사용된 경우 `std::bad_cast` 예외가 발생합니다.