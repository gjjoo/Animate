# Animate
Sass를 위한 Animate Custom 자료입니다.<br>
`geoffgraham`의 animatewithsass를 보완한 자료입니다.

```
Animate: https://github.com/daneden/animate.css
Animate for Sass: https://github.com/geoffgraham/animate.scss
```
<br>

:one: 네임스페이스 기능을 추가했습니다.
```sass
  /* ~~~ _properties.scss ~~~ */
  $aniNameSpace: 'ani' !default;`

  /* ~~~ _animate.scss ~~~ */
  /* ... */
  .#{$aniNameSpace}-bounce {
    @include bounce();
  }
  .#{$aniNameSpace}-flash {
    @include flash();
  }
  /* ... */
```
<br>

:two: IncludeClasses 변수를 통해 CSS파일의 클래스로써의 사용여부를 변경할 수 있습니다.
```sass
  /* ~~~ _properties.scss ~~~ */
  $aniIncludeClasses: true !default;

  /* ~~~ _animate.scss ~~~ */
  @if $aniIncludeClasses {
    .#{$aniNameSpace}-bounce {
      @include bounce();
    }
    /* ... */
  }
```
<br>

:three: 해당 변수를 이용함으로써 CSS파일 클래스를 사용하지 않고 Sass의 Mixin을 이용해서 원하는 클래스의 속성에만 코드를 넣을 수 있습니다. animate 기능을 많이 사용하지 않을 시 코드량을 줄여줍니다.
```sass
  .your-class-name {
    @include bounceIn();
  }
```
<br>

:four: 기존 `geoffgraham의 animatewithsass`는 클래스포함여부 상관없이 모든 키프레임을 CSS로 출력하게끔 되어 있었습니다. 사용한 Mixin의 keyframe만 출력하게 하고 중복으로 사용시에도 keyframe이 한번 만 출력되게끔 수정했습니다.
```sass
$animate-bounce: true; /* +Add */
@mixin bounce($count: $countDefault, $duration: $durationDefault, $delay: $delayDefault, $function: $functionDefault, $fill: $fillDefault, $visibility: $visibilityDefault) {
  @include animation-name(#{$aniNameSpace}-bounce); /* +Add: 네임스페이스 기능 추가 */
  @include count($count);
  @include duration($duration);
  @include delay($delay);
  @include function($function);
  @include fill-mode($fill);
  @include visibility($visibility);
  @if $animate-bounce { /* +Add */
    $animate-bounce: false !global; /* +Add */
    @include keyframes(#{$aniNameSpace}-bounce) { /* +Add: 네임스페이스 기능 추가 */
      0%, 20%, 50%, 80%, 100% {@include transform(translateY(0));}
      40% {@include transform(translateY(-30px));}
      60% {@include transform(translateY(-15px));}
    }
  }
}
```