<!-- .element data-auto-animate -->
## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {
    // Implement me.
};
</code></pre>

@@@
<!-- .element data-auto-animate -->

## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {

  friend bool operator==(const T& lhs, const T& rhs) {
    // Implement me.
  }

  friend bool operator!=(const T& lhs, const T& rhs) {
    return !(lhs == rhs);
  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->

## Implementation: Extensions

<pre data-id="eq-animation"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct EqualityExtension : Extension&lt;EqualityExtension, T&gt; {

  friend bool operator==(const T& lhs, const T& rhs) {
    return EqualityExtension::Unpack(lhs) ==
             EqualityExtension::Unpack(rhs);
  }

  friend bool operator!=(const T& lhs, const T& rhs) {
    return !(lhs == rhs);
  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->
## Implementation: Extensions

<pre data-id="serial-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct SerializeExtension : Extension&lt;SerializeExtension, T&gt; {

  friend void Serialize(Serializer& s, const T& value) {
    // Implement me.
  }

  friend bool Deserialize(Deserializer& d, T& value) {
    // Implement me.
  }

};
</code></pre>

@@@
<!-- .element data-auto-animate -->
## Implementation: Extensions

<pre data-id="serial-animation" style="font-size:14pt;"><code data-trim data-line-numbers>
template &lt;typename T&gt;
struct SerializeExtension : Extension&lt;SerializeExtension, T&gt; {

  friend void Serialize(Serializer& s, const T& value) {
    std::apply([&] auto&... fields) {
      (s.Serialize(fields), ...);
    }, SerializeExtension::Unpack(*this));
  }

  friend bool Deserialize(Deserializer& d, T& value) {
    return std::apply([&] (auto&... fields) {
      return (d.Deserialize(fields) && ...);
    }, SerializeExtension::Unpack(*this));
  }

};
</code></pre>
