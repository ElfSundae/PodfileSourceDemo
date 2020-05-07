# Demo for Podfile `source` issue

https://github.com/CocoaPods/CocoaPods/issues/9457#issuecomment-573069810

There are two issues about the `source` directive:

\#1 The scope of the `:source` parameter in `pod` method, is not local (just for this `pod` method), but global.

```ruby
source 'https://cdn.cocoapods.org/'

platform :ios, '9.0'

target 'PodSourceDemo' do
    pod 'AFNetworking'
    pod 'SocketRocket', :source => 'https://github.com/ElfSundae/CocoaPods-Specs.git'
end
```

`AFNetworking` will be installed from `ElfSundae/CocoaPods-Specs` too.

> **By default** the sources specified at **the global level are searched** in the order they are specified for a dependency match.
>
> <cite>via ["Source" section of the `pod` syntax reference](https://guides.cocoapods.org/syntax/podfile.html#pod)</cite>

\#2 The order of the sources is irrelevant.

```ruby
source 'https://cdn.cocoapods.org/'
source 'https://github.com/ElfSundae/CocoaPods-Specs.git'

platform :ios, '9.0'

target 'PodSourceDemo' do
    pod 'AFNetworking'
end
```

`AFNetworking` will be installed from `ElfSundae/CocoaPods-Specs`.

> The order of the sources is **relevant**. CocoaPods will use the highest version of a Pod of the first source which includes the Pod (**regardless whether other sources have a higher version**).
>
> <cite>via [`source` syntax reference](https://guides.cocoapods.org/syntax/podfile.html#source)</cite>
