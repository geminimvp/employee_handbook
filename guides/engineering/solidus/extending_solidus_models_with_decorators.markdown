# Extending Solidus Models with Decorators

Occasionally, we will want to add some functionality to the models in Solidus. However, we don't want to bring the models themselves into [engine_storefront](https://github.com/geminimvp/engine_storefront) because we don't want to take complete responsibility for the models.

# The decorator pattern

Decorators are used to extend a class without affecting all instances of the class. This is similar in some respects to creating sub-classes. [Useful blog post](http://nithinbekal.com/posts/ruby-decorators/).

This is specifically useful concerning Solidus because it lets us easily add functionality without much overhead. The implementation of this pattern been [documented in the Solidus guides](https://guides.solidus.io/developers/extensions/decorators.html). 

# When to use a decorator in Engine Storefront

The best use case for using decorators in Engine Storefront is when we need to implement model level logic for our platform that may not be useful in the Solidus platform. We may also want to implement a decorator if we plan to keep a feature out of Solidus to help us differentiate Engine, or if we are testing an idea for validity before trying to put it back into Solidus.
