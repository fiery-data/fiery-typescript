
Example (something along the lines of this)

- Sub arrays/maps/streams
- Document reference fields
- Lazy initialization

```typescript
import { Type, Record, Transaction, SubCollection, Collection, Field } from 'fiery-typescript';
import { $fiery } from './local/file';

@Type({
  $fiery // options
})
export class Task extends Record<Task> {
  
  @Field() // include
  public name: string;
  
  @DocumentField() // DocumentReference field type
  public parent: Task;
  
  @SubCollection('subtasks', () => Task, {
    once: true // options
  })
  public subtasks: Collection<Task>;
  
  @Transaction
  public async transactionAction() {
  
  }
  
}

Task.$doc('path/to/doc'); // Task; real-time doc
Task.$collection('path/to/collection'); // Collection<Task>; 

const x = Task.$doc('tasks/1234');
const y = x.subtasks.$create({ name: 'Subtask });

class Record<T> {
   public $ref: firebase.firestore.DocumentReference;
   public $fiery: Fiery;
   public $id: string;
   public $exists: boolean;
   
   public $free(): Promise<void>;
   public $entry(): FieryEntry;
   
   public async $refresh(cachedOnly?: boolean): Promise<void>;
   public async $update(fields?: FieryFields): Promise<void>;
   public async $save(fields?: FieryFields): Promise<void>;
   public async $sync(fields?: FieryFields): Promise<void>;
   public async $remove(excludeSubs?: boolean): Promise<void>;
   public async $clear(sub: propof T): Promise<void>;
   public <K extends keyof T, E extends (T[K] extends Collection<infer S> ? S : never)> $create(K: sub, initial?: Partial<E>): E;
   public <K extends keyof T, E extends (T[K] extends Collection<infer S> ? S : never)> $build(K: sub, initial?: Partial<E>): E;
       
   public $onError(error: any): void;
   public $onSuccess(target: T): void;
   public $onMissing(): void;
   public $onRemove(): void;
   public $onMutate(mutator: () => T): void;
   public $onPromise(promise: Promise<T>): void;
   
   public static async $doc(id: string, options?: FieryOptions): T;
   public static async $collection(id: string, options?: FieryOptions): Collection<T>;
}

type Collection<T> = {
  $free(): Promise<void>;
  $entry(): FieryEntry;
  $pager(): FieryPager;
  $more(count?: number): Promise<void>;
  $hasMore(): boolean;
  $create(initial?: Partial<T>): T;
  $build(initial?: Partial<T>): T;
};

type Array<T> = T[] & Collection<T>;
type Map<T> = { [id: string]: T } & Collection<T>;





```
