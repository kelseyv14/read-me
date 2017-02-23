
> Objects are frequently used in JavaScript, and understanding how to work with them effectively will be a huge win for your productivity. In fact, poor OO design can potentially lead to project failure, and in the worst cases, company failures.
##so far, so good. I feel like I have a pretty good working knowledge of objects##

#prototypes, not classes# 

^so far my favorite thing about JS^

>how to put it to :sparkles: best :sparkles: use 

---love it. what is an example of "good, better, or best" use of JS

>Class Inheritance: A class is like a blueprint — a description of the object to be created. Classes inherit from classes and create subclass relationships: hierarchical class taxonomies.

---more serendipity in coding :v:

>“Favor object composition over class inheritance.”

---does this mean...make an object that has consise variables, doesn't repeat when not nescessary, excludes in the ES6 format...etc ? 


-------need to learn how to "fork" in code...clearly...


HTML  Babel  Result
EDIT ON
 // Class Inheritance Example
// NOT RECOMMENDED. Use object composition, instead.

// https://gist.github.com/ericelliott/b668ce0ad1ab540df915
// http://codepen.io/ericelliott/pen/pgdPOb?editors=001

//this is the head of the object. I'm not sure if that is the accurate term, but to me it represents the first piece and the name of the object

class GuitarAmp {
  constructor ({ cabinet = 'spruce', distortion = '1', volume = '0' } = {}) {
    Object.assign(this, {
      cabinet, distortion, volume
    });
  }
}

// here's the first extension, I'm starting to see why this is not reccomended...already there is repeating

class BassAmp extends GuitarAmp {
  constructor (options = {}) {
    super(options);
    this.lowCut = options.lowCut;
  }
}

// now we deviate even further another extension with more repetition and not a ton of definition. it's logical, but not very practical


class ChannelStrip extends BassAmp {
  constructor (options = {}) {
    super(options);
    this.inputLevel = options.inputLevel;
  }
}
//nesting 
test('Class Inheritance', nest => {
  nest.test('BassAmp', assert => {
    const msg = `instance should inherit props
    from GuitarAmp and BassAmp`;

    const myAmp = new BassAmp();
    const actual = Object.keys(myAmp);
    const expected = ['cabinet', 'distortion', 'volume', 'lowCut'];

    assert.deepEqual(actual, expected, msg);
    assert.end();
  });
//a lot of code for not a ton going on


  nest.test('ChannelStrip', assert => {
    const msg = 'instance should inherit from GuitarAmp, BassAmp, and ChannelStrip';
    const myStrip = new ChannelStrip();
    const actual = Object.keys(myStrip);
    const expected = ['cabinet', 'distortion', 'volume', 'lowCut', 'inputLevel'];

    assert.deepEqual(actual, expected, msg);
    assert.end();
  });
});



// Composition Example, more reccomended

// http://codepen.io/ericelliott/pen/XXzadQ?editors=001
// https://gist.github.com/ericelliott/fed0fd7a0d3388b06402


//here we have practical variables on top of the object, giving us reference to an object that doesn't exisit yet. I see this as helpful for your future coder self to give yourself an anchor to use as reference. you know what you want your object to do from the beginning.


const distortion = { distortion: 1 };
const volume = { volume: 1 };
const cabinet = { cabinet: 'maple' };
const lowCut = { lowCut: 1 };
const inputLevel = { inputLevel: 1 };

//wow an object ! so petite, so consise ! good javascript ?


const GuitarAmp = (options) => {
  return Object.assign({}, distortion, volume, cabinet, options);
};

//and another ?! no extension

const BassAmp = (options) => {
  return Object.assign({}, lowCut, volume, cabinet, options);
};

const ChannelStrip = (options) => {
  return Object.assign({}, inputLevel, lowCut, volume, options);
};


test('GuitarAmp', assert => {
  const msg = 'should have distortion, volume, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = GuitarAmp({
    distortion: level,
    volume: level,
    cabinet
  });
  const expected = {
    distortion: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});

test('BassAmp', assert => {
  const msg = 'should have volume, lowCut, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = BassAmp({
    lowCut: level,
    volume: level,
    cabinet
  });
  const expected = {
    lowCut: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});


test('ChannelStrip', assert => {
  const msg = 'should have inputLevel, lowCut, and volume';
  const level = 2;

  const actual = ChannelStrip({
    inputLevel: level,
    lowCut: level,
    volume: level
  });
  const expected = {
    inputLevel: level,
    lowCut: level,
    volume: level
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});